# 12. SVD를 활용한 데이터 압축 (Data Compression with SVD)

> **"이미지 데이터를 96% 줄이면서 품질은 유지하기"**

이번 장에서는 **SVD(특이값 분해)**를 활용하여 이미지 데이터를 **극적으로 압축**하는 실습을 진행합니다. 이 기법은 이미지뿐만 아니라 다양한 행렬 데이터의 **손실 압축(Lossy Compression)**에 활용됩니다.

_이 장의 코드는 Frank Cleary의 코드를 참고했습니다._

---

## 1️⃣ 압축 원리 이해하기

### SVD 복습

$$
A = U D V^T
$$

- $A$: 원본 행렬 ($m \times n$)
- $U$: 좌특이벡터 ($m \times m$)
- $D$: 특이값 대각행렬 ($m \times n$)
- $V^T$: 우특이벡터 전치 ($n \times n$)

### 핵심 아이디어: 상위 k개만 사용하기

특이값은 **내림차순**으로 정렬되어 있습니다:

$$
\sigma_1 \geq \sigma_2 \geq \sigma_3 \geq \dots \geq \sigma_n \geq 0
$$

**첫 번째 특이값/벡터가 가장 중요한 정보**를 담고 있습니다!

따라서 상위 $k$개의 특이값/벡터만 사용해도 원본을 **근사(approximate)**할 수 있습니다:

$$
A \approx U_k D_k V_k^T
$$

여기서:

- $U_k$: 처음 $k$개의 좌특이벡터 ($m \times k$)
- $D_k$: 처음 $k$개의 특이값 ($k \times k$)
- $V_k^T$: 처음 $k$개의 우특이벡터 ($k \times n$)

---

## 2️⃣ 이미지 불러오기 및 전처리

### 필요한 라이브러리 임포트

```python
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
import requests
from io import BytesIO
```

### 이미지 불러오기

```python
# 이미지 URL에서 불러오기 (예시)
url = "https://example.com/your_image.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

# 또는 로컬 파일에서 불러오기
# img = Image.open("your_image.jpg")

# 이미지 표시
plt.imshow(img)
plt.axis('off')
plt.title("원본 이미지")
plt.show()
```

### 그레이스케일 변환

**왜 그레이스케일?**

- 컬러 이미지는 RGB 3개 채널 → 3차원 텐서
- 그레이스케일은 1개 채널 → 2차원 행렬
- SVD 학습에 더 적합!

```python
# 그레이스케일로 변환
img_gray = img.convert('L')

# 그레이스케일 이미지 표시
plt.imshow(img_gray, cmap='gray')
plt.axis('off')
plt.title("그레이스케일 이미지")
plt.show()
```

### NumPy 행렬로 변환

```python
# 이미지를 NumPy 배열로 변환
img_array = np.array(img_gray, dtype=np.float64)

print(f"이미지 행렬 shape: {img_array.shape}")
# 예시 출력: (4032, 3024) → 4032행 × 3024열

print(f"총 픽셀 수: {img_array.shape[0] * img_array.shape[1]:,}")
# 예시 출력: 12,192,768개
```

**각 요소의 의미**: 해당 픽셀이 얼마나 어두운지 (0=검정, 255=흰색)

---

## 3️⃣ SVD 적용하기

```python
# SVD 수행
U, d, Vt = np.linalg.svd(img_array)

print(f"U shape: {U.shape}")      # (4032, 4032) - 좌특이벡터
print(f"d shape: {d.shape}")      # (3024,) - 특이값 벡터
print(f"Vt shape: {Vt.shape}")    # (3024, 3024) - 우특이벡터 전치
```

**참고**: 이미지 크기가 크면 (예: 4000×3000) 계산에 몇 초 걸릴 수 있습니다.

---

## 4️⃣ 상위 k개 특이값으로 이미지 재구성

### 단일 특이값으로 재구성 (k=1)

```python
# 상위 k개만 사용
k = 1

# 상위 k개 추출
U_k = U[:, :k]           # 처음 k개 좌특이벡터 (m × k)
d_k = d[:k]              # 처음 k개 특이값 (k,)
Vt_k = Vt[:k, :]         # 처음 k개 우특이벡터 (k × n)

# 대각행렬 생성
D_k = np.diag(d_k)       # (k × k)

# 이미지 재구성: U_k × D_k × Vt_k
img_reconstructed = U_k @ D_k @ Vt_k

# 결과 시각화
plt.imshow(img_reconstructed, cmap='gray')
plt.axis('off')
plt.title(f'k={k} 특이값으로 재구성')
plt.show()
```

### 다양한 k값으로 재구성 비교

```python
# 다양한 k값 테스트
k_values = [1, 2, 4, 8, 16, 32, 64]

fig, axes = plt.subplots(2, 4, figsize=(16, 8))
axes = axes.flatten()

# 원본 이미지
axes[0].imshow(img_array, cmap='gray')
axes[0].set_title('원본')
axes[0].axis('off')

# 다양한 k값으로 재구성
for i, k in enumerate(k_values):
    # 상위 k개 추출
    U_k = U[:, :k]
    d_k = d[:k]
    Vt_k = Vt[:k, :]

    # 재구성
    img_reconstructed = U_k @ np.diag(d_k) @ Vt_k

    # 시각화
    axes[i+1].imshow(img_reconstructed, cmap='gray')
    axes[i+1].set_title(f'k={k}')
    axes[i+1].axis('off')

plt.tight_layout()
plt.show()
```

### 결과 해석

| k값      | 이미지 품질                    |
| :------- | :----------------------------- |
| **k=1**  | 형체를 알아볼 수 없음          |
| **k=2**  | 희미한 윤곽이 보이기 시작      |
| **k=4**  | "강아지인가? 아기곰인가?" 수준 |
| **k=8**  | 형태가 뚜렷해짐                |
| **k=16** | 상당히 괜찮은 품질             |
| **k=32** | 거의 원본 수준                 |
| **k=64** | 원본과 구분이 어려움           |

---

## 5️⃣ 압축률 계산하기

### 원본 데이터 크기

```python
m, n = img_array.shape
original_size = m * n

print(f"원본 이미지: {m} × {n}")
print(f"원본 데이터 개수: {original_size:,}")
# 예시: 4032 × 3024 = 12,192,768개
```

### 압축된 데이터 크기

k개의 특이값을 사용할 때 저장해야 하는 데이터:

- $U_k$: $m \times k$ 개
- $d_k$: $k$ 개
- $V_k^T$: $k \times n$ 개

```python
k = 64

# 압축된 데이터 크기
compressed_size = (m * k) + k + (k * n)
# U_k: m*k, d_k: k, Vt_k: k*n

print(f"\n=== k={k} 사용 시 ===")
print(f"U_k 크기: {m} × {k} = {m * k:,}")
print(f"d_k 크기: {k}")
print(f"Vt_k 크기: {k} × {n} = {k * n:,}")
print(f"압축된 총 크기: {compressed_size:,}")
```

### 압축률 계산

```python
compression_ratio = compressed_size / original_size * 100
savings = 100 - compression_ratio

print(f"\n=== 압축 결과 ===")
print(f"원본 크기: {original_size:,}")
print(f"압축 크기: {compressed_size:,}")
print(f"압축률: {compression_ratio:.2f}%")
print(f"절약률: {savings:.2f}%")
```

**예시 결과 (k=64)**:

```
원본 크기: 12,192,768
압축 크기: 451,648
압축률: 3.70%
절약률: 96.30%
```

🎉 **96% 이상의 데이터를 절약**하면서도 이미지 품질을 유지할 수 있습니다!

---

## 6️⃣ 압축률 vs 품질 분석

```python
# 다양한 k값에 대한 압축률 계산
k_values = [1, 2, 4, 8, 16, 32, 64, 128, 256]
compression_ratios = []

for k in k_values:
    compressed_size = (m * k) + k + (k * n)
    ratio = compressed_size / original_size * 100
    compression_ratios.append(ratio)
    print(f"k={k:3d}: 압축률 {ratio:5.2f}%, 절약률 {100-ratio:5.2f}%")

# 시각화
plt.figure(figsize=(10, 5))
plt.plot(k_values, compression_ratios, 'bo-', linewidth=2, markersize=8)
plt.xlabel('사용한 특이값 개수 (k)')
plt.ylabel('압축률 (%)')
plt.title('특이값 개수에 따른 압축률')
plt.grid(True, alpha=0.3)
plt.xscale('log', base=2)
plt.show()
```

---

## 7️⃣ 전체 코드 요약

```python
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt

# 1. 이미지 로드 및 그레이스케일 변환
img = Image.open("your_image.jpg").convert('L')
img_array = np.array(img, dtype=np.float64)

# 2. SVD 수행
U, d, Vt = np.linalg.svd(img_array)

# 3. 상위 k개로 재구성
k = 64
U_k = U[:, :k]
d_k = d[:k]
Vt_k = Vt[:k, :]
img_compressed = U_k @ np.diag(d_k) @ Vt_k

# 4. 압축률 계산
m, n = img_array.shape
original = m * n
compressed = (m * k) + k + (k * n)
print(f"압축률: {compressed/original*100:.2f}%")
print(f"절약률: {(1 - compressed/original)*100:.2f}%")

# 5. 결과 비교
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 6))
ax1.imshow(img_array, cmap='gray')
ax1.set_title('원본')
ax2.imshow(img_compressed, cmap='gray')
ax2.set_title(f'압축 (k={k})')
plt.show()
```

---

## 🎯 요약

### SVD 압축의 핵심

| 단계                 | 설명                      |
| :------------------- | :------------------------ |
| **1. SVD 분해**      | $A = U D V^T$             |
| **2. 상위 k개 선택** | 첫 k개 특이값/벡터만 추출 |
| **3. 근사 재구성**   | $A \approx U_k D_k V_k^T$ |
| **4. 압축 효과**     | 저장 공간 대폭 감소       |

### 압축 공식

$$
\text{압축 크기} = (m \times k) + k + (k \times n)
$$

$$
\text{압축률} = \frac{\text{압축 크기}}{\text{원본 크기}} \times 100\%
$$

### 실용적 가이드

| 용도            | 권장 k 값 | 압축률  |
| :-------------- | :-------- | :------ |
| 썸네일/미리보기 | 8~16      | ~1%     |
| 일반 용도       | 32~64     | ~3~5%   |
| 고품질 보존     | 128~256   | ~10~20% |

### 머신러닝에서의 활용

1. **입력 데이터 축소**: 모델 입력 크기 감소 → 연산 효율화
2. **노이즈 제거**: 낮은 특이값 = 노이즈, 제거하면 품질 향상
3. **특성 추출**: 상위 특이벡터 = 데이터의 주요 패턴
4. **저장 공간 절약**: 대용량 데이터셋 압축

---

## 다음 단계

다음 영상에서는 SVD를 기반으로 한 **무어-펜로즈 역행렬(Moore-Penrose Pseudoinverse)**을 배웁니다. 역행렬이 존재하지 않는 행렬에서도 선형방정식의 해를 구할 수 있는 "마법 같은" 기법입니다!
