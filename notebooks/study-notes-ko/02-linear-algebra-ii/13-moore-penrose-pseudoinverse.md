# 13. 무어-펜로즈 역행렬 (Moore-Penrose Pseudoinverse)

> **"역행렬이 없는 행렬도 '역치'할 수 있다!"**

이번 장에서는 **무어-펜로즈 역행렬(유사역행렬, Pseudoinverse)**을 배웁니다. 비정방행렬이나 특이행렬처럼 역행렬이 존재하지 않는 경우에도 **연립방정식의 해를 구할 수 있게 해주는** 강력한 도구입니다.

---

## 1️⃣ 왜 일반 역행렬이 안 되는가?

### 역행렬의 조건 (복습)

강좌 초반에 배웠듯이, 역행렬이 존재하려면 다음 조건을 **모두** 만족해야 합니다:

| 조건                | 설명                            |
| :------------------ | :------------------------------ |
| **정방행렬**        | 행 수 = 열 수 ($n \times n$)    |
| **과결정 없음**     | 방정식 수 ≤ 미지수 수           |
| **불충분결정 없음** | 방정식 수 ≥ 미지수 수           |
| **비특이**          | 모든 열이 일차독립 (행렬식 ≠ 0) |

### 문제 상황들

#### 1. 과결정 (Overdetermined): 방정식 > 미지수

```
3개의 방정식, 2개의 미지수 (3×2 행렬)
→ 교차점이 여러 개 → 단일 해 없음
```

```python
# 과결정 행렬 예시
A = np.array([[1, 2],
              [3, 4],
              [5, 6]])  # 3행 2열
# np.linalg.inv(A) → 에러! (정방행렬 아님)
```

#### 2. 불충분결정 (Underdetermined): 방정식 < 미지수

```
1개의 방정식, 2개의 미지수 (1×2 행렬)
→ 교차점이 없거나 무한대 → 단일 해 없음
```

#### 3. 특이행렬 (Singular): 일차종속

```
열이 서로 비례 (예: [1,2]와 [2,4])
→ 평행선 → 교차점 없음 or 무한대
```

```python
# 특이행렬 예시
A = np.array([[1, 2],
              [2, 4]])  # 두 열이 비례
np.linalg.det(A)  # 0.0
# np.linalg.inv(A) → 에러! (Singular matrix)
```

### 해결책: 무어-펜로즈 역행렬!

위 모든 경우에서 역행렬 대신 **유사역행렬(Pseudoinverse)**을 사용할 수 있습니다.

---

## 2️⃣ 무어-펜로즈 역행렬 공식

### 정의

행렬 $A$의 무어-펜로즈 역행렬(유사역행렬)은 $A^+$로 표기하며, 다음과 같이 정의됩니다:

$$
A^+ = V D^+ U^T
$$

여기서:

- $U, D, V$: $A$의 **SVD** 결과 ($A = U D V^T$)
- $D^+$: $D$의 **유사역행렬**

### D⁺ 계산 방법

$D^+$는 다음 단계로 계산합니다:

1. $D$의 **0이 아닌 요소**들의 **역수**를 구함
2. 결과 행렬을 **전치**

$$
D = \begin{bmatrix} \sigma_1 & 0 \\ 0 & \sigma_2 \\ 0 & 0 \end{bmatrix}
\quad \Rightarrow \quad
D^+ = \begin{bmatrix} 1/\sigma_1 & 0 & 0 \\ 0 & 1/\sigma_2 & 0 \end{bmatrix}
$$

**주의**: $D$가 $m \times n$이면, $D^+$는 $n \times m$입니다!

---

## 3️⃣ NumPy로 유사역행렬 구하기 (수동 계산)

### 예제 행렬

```python
import numpy as np

# 비정방행렬 (3×2) - 역행렬 불가
A = np.array([[-1, 2],
              [3, -2],
              [5, 7]])

print(f"행렬 A (shape: {A.shape}):")
print(A)
```

### 1단계: SVD 수행

```python
# SVD 분해
U, d, Vt = np.linalg.svd(A)

print(f"\nU (좌특이벡터, shape: {U.shape}):")
print(U)

print(f"\n특이값 d: {d}")
# 출력: [8.66916085 4.10429538]

print(f"\nVt (우특이벡터 전치, shape: {Vt.shape}):")
print(Vt)
```

### 2단계: D⁺ 계산

```python
# D (대각행렬) 생성
D = np.zeros((A.shape[0], A.shape[1]))  # 3×2
np.fill_diagonal(D, d)
print(f"D:\n{D}")

# 0이 아닌 요소의 역수 계산
D_inv = np.diag(1 / d)  # 2×2 대각행렬
print(f"\nD의 역행렬 (비영 요소):\n{D_inv}")
# 출력:
# [[0.11535108 0.        ]
#  [0.         0.24364732]]

# D⁺: 역행렬을 전치 → 차원 맞추기
# D가 3×2이므로, D⁺는 2×3이어야 함
D_plus = np.zeros((A.shape[1], A.shape[0]))  # 2×3
D_plus[:D_inv.shape[0], :D_inv.shape[1]] = D_inv
print(f"\nD⁺ (shape: {D_plus.shape}):")
print(D_plus)
```

**참고**: $D$가 대칭인 경우(정방행렬 부분), 역행렬을 구한 후 전치하면 됩니다.

### 3단계: A⁺ 계산

$$
A^+ = V D^+ U^T
$$

```python
# V 구하기 (Vt의 전치)
V = Vt.T
print(f"V:\n{V}")

# U 전치
Ut = U.T
print(f"\nU 전치:\n{Ut}")

# A⁺ = V @ D⁺ @ Uᵀ
A_plus = V @ D_plus @ Ut
print(f"\nA⁺ (유사역행렬, shape: {A_plus.shape}):")
print(A_plus)
```

### 4단계: 검증

유사역행렬이 올바른지 확인하는 방법:

```python
# A × A⁺ × A ≈ A (Moore-Penrose 조건 중 하나)
check = A @ A_plus @ A
print(f"\nA × A⁺ × A:\n{check}")
print(f"\n원본 A와 같은가?: {np.allclose(A, check)}")
# 출력: True ✅
```

---

## 4️⃣ NumPy의 pinv 메서드 (간편한 방법)

위의 모든 과정을 **한 줄**로 할 수 있습니다!

```python
# np.linalg.pinv: Pseudoinverse 계산
A_plus_easy = np.linalg.pinv(A)
print(f"np.linalg.pinv(A):\n{A_plus_easy}")

# 수동 계산과 동일한지 확인
print(f"\n수동 계산과 동일?: {np.allclose(A_plus, A_plus_easy)}")
# 출력: True ✅
```

### pinv vs inv 비교

| 메서드              | 용도          | 적용 가능 행렬       |
| :------------------ | :------------ | :------------------- |
| `np.linalg.inv(A)`  | 정확한 역행렬 | 정방 + 비특이 행렬만 |
| `np.linalg.pinv(A)` | 유사역행렬    | **모든 행렬**        |

---

## 5️⃣ 전체 코드 요약

### 수동 계산 버전

```python
import numpy as np

# 행렬 정의
A = np.array([[-1, 2], [3, -2], [5, 7]])

# 1. SVD 분해
U, d, Vt = np.linalg.svd(A)

# 2. D⁺ 계산
D_inv = np.diag(1 / d)
D_plus = np.zeros((A.shape[1], A.shape[0]))
D_plus[:len(d), :len(d)] = D_inv

# 3. A⁺ = V D⁺ Uᵀ
V = Vt.T
A_plus = V @ D_plus @ U.T

print("유사역행렬 A⁺:")
print(A_plus)
```

### 간편 버전

```python
import numpy as np

A = np.array([[-1, 2], [3, -2], [5, 7]])
A_plus = np.linalg.pinv(A)
print("유사역행렬 A⁺:")
print(A_plus)
```

---

## 6️⃣ PyTorch 연습 문제

### 문제 1: SVD를 이용한 유사역행렬 계산

행렬 $A_P$에 대해 PyTorch로 SVD를 수행하고, 수동으로 유사역행렬을 계산하세요.

```python
import torch

# 행렬 정의
A_P = torch.tensor([[1., 2., 3.],
                    [4., 5., 6.]])

# TODO: torch.linalg.svd 사용하여 U, S, Vt 구하기

# TODO: D⁺ 계산 (S의 역수 + 차원 맞추기)

# TODO: A⁺ = V @ D⁺ @ Uᵀ 계산
```

### 문제 2: torch.linalg.pinv 사용

```python
# torch.linalg.pinv로 간편하게 계산
A_plus_torch = torch.linalg.pinv(A_P)
print(A_plus_torch)

# 수동 계산 결과와 비교
```

### 힌트

- PyTorch SVD: `torch.linalg.svd(A)` → U, S, Vh 반환
- 역수: `1 / S`
- 대각행렬: `torch.diag(S)`
- 전치: `.T` 또는 `.mT`
- 유사역행렬: `torch.linalg.pinv(A)`

---

## 🎯 요약

### 핵심 공식

$$
A^+ = V D^+ U^T
$$

여기서:

- $U, D, V$는 $A = UDV^T$ (SVD 결과)
- $D^+$는 $D$의 비영 요소 역수 + 전치

### 단계별 정리

| 단계 | 작업                                     |
| :--- | :--------------------------------------- |
| 1    | SVD 수행: $A = UDV^T$                    |
| 2    | $D$의 0 아닌 요소들의 역수 계산          |
| 3    | 결과를 전치하여 $D^+$ 생성 (차원 맞추기) |
| 4    | $A^+ = V D^+ U^T$ 계산                   |

### 실용 팁

```python
# 복잡한 수동 계산 대신...
A_plus = np.linalg.pinv(A)  # NumPy
A_plus = torch.linalg.pinv(A)  # PyTorch
```

### 왜 중요한가?

- **과결정 시스템**: 최소제곱해 (최적의 근사해)
- **불충분결정 시스템**: 최소 노름해 (가장 작은 크기의 해)
- **머신러닝**: 선형 회귀, 데이터 피팅의 핵심 도구

---

## 다음 단계

다음 영상에서는 무어-펜로즈 역행렬을 활용하여:

1. **연립방정식의 미지수** 풀기
2. **직선을 데이터 좌표에 피팅**하기 (선형 회귀의 핵심!)

를 배워봅니다. 이것이 바로 머신러닝에서 유사역행렬이 사용되는 실제 사례입니다!
