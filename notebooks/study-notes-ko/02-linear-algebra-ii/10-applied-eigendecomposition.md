# 10. 고유값 분해의 실제 적용 사례 (Applied Eigendecomposition)

> **"고유값과 고유벡터는 어디에 쓰이는가?"**

이번 장에서는 고유값(Eigenvalue)과 고유벡터(Eigenvector)가 **머신러닝**과 **실제 세계**에서 어떻게 활용되는지 알아봅니다. 지금까지 배운 이론을 실제 응용 사례와 연결하여 이해를 완성합니다.

---

## 1️⃣ 2차원 기하학 변형 복습

고유값 분해 파트의 초반에 배웠던 **아핀 변형(Affine Transformation)**을 다시 살펴봅시다. 이제 고유값/고유벡터 개념을 바탕으로 더 깊이 이해할 수 있습니다.

### 1.1 균일 스케일링 (Uniform Scaling)

모든 축에 **동일한 비율** $K$로 확대/축소하는 변환:

$$
\text{Scale} = \begin{bmatrix} K & 0 \\ 0 & K \end{bmatrix}
$$

**고유값과 고유벡터**:

- **고유값**: 둘 다 $K$ (동일한 스케일 팩터)
- **고유벡터**: $\begin{bmatrix} 1 \\ 0 \end{bmatrix}$, $\begin{bmatrix} 0 \\ 1 \end{bmatrix}$ (x축, y축 방향)

```python
import numpy as np

K = 2  # 스케일 팩터
Scale = np.array([[K, 0],
                  [0, K]])

lambdas, V = np.linalg.eig(Scale)
print(f"고유값: {lambdas}")      # [2. 2.]
print(f"고유벡터:\n{V}")
# [[1. 0.]
#  [0. 1.]]
```

### 1.2 비균일 스케일링 (Non-Uniform Scaling)

x축과 y축에 **다른 비율**로 확대/축소하는 변환:

$$
\text{Scale} = \begin{bmatrix} K_1 & 0 \\ 0 & K_2 \end{bmatrix}
$$

**고유값과 고유벡터**:

- **고유값**: $K_1$, $K_2$ (각 축별 스케일 팩터)
- **고유벡터**: x축, y축 방향 벡터

```python
K1, K2 = 3, 0.5  # x축 3배, y축 절반
Scale = np.array([[K1, 0],
                  [0, K2]])

lambdas, V = np.linalg.eig(Scale)
print(f"고유값: {lambdas}")      # [3.  0.5]
print(f"고유벡터:\n{V}")
# [[1. 0.]
#  [0. 1.]]
```

### 1.3 밀림 변환 (Shear Transformation)

모나리자 예시를 기억하시나요? 한 방향으로 "밀리는" 변환입니다.

#### 수평 밀림 (Horizontal Shear)

$$
\text{Shear}_H = \begin{bmatrix} 1 & s \\ 0 & 1 \end{bmatrix}
$$

```python
s = 0.5  # 밀림 강도
Shear_H = np.array([[1, s],
                    [0, 1]])

lambdas, V = np.linalg.eig(Shear_H)
print(f"고유값: {lambdas}")      # [1. 1.] ← 모두 1!
print(f"고유벡터:\n{V}")
# [[1.         0.        ]  ← x축 방향만 고유벡터
#  [0.         1.        ]]
```

**기하학적 의미**:

- 수평 밀림에서는 **수직 방향(y축) 벡터**가 스팬을 잃습니다
- **수평 방향(x축) 벡터**만이 고유벡터가 됩니다 (방향 유지)

#### 수직 밀림 (Vertical Shear)

$$
\text{Shear}_V = \begin{bmatrix} 1 & 0 \\ s & 1 \end{bmatrix}
$$

```python
s = 0.5
Shear_V = np.array([[1, 0],
                    [s, 1]])

lambdas, V = np.linalg.eig(Shear_V)
print(f"고유값: {lambdas}")      # [1. 1.]
# 이번에는 y축 방향이 고유벡터
```

**핵심 포인트**: 밀림 변환에서 고유값은 항상 1이지만, 고유벡터는 밀림 방향에 수직인 벡터입니다.

---

## 2️⃣ 행렬의 확정성 (Definiteness)

행렬의 고유값 부호에 따라 행렬을 분류할 수 있습니다. 이 개념은 머신러닝 문헌에서 자주 등장합니다.

### 행렬 확정성 분류표

| 분류                                       | 고유값 조건             | 의미                                   |
| :----------------------------------------- | :---------------------- | :------------------------------------- |
| **양의 정부호 (Positive Definite)**        | 모든 $\lambda > 0$      | 공간을 확장 (부피 증가)                |
| **양의 준정부호 (Positive Semi-Definite)** | 모든 $\lambda \geq 0$   | 확장 또는 유지 (일부 차원 붕괴 가능)   |
| **음의 정부호 (Negative Definite)**        | 모든 $\lambda < 0$      | 공간을 뒤집고 축소                     |
| **음의 준정부호 (Negative Semi-Definite)** | 모든 $\lambda \leq 0$   | 뒤집기 또는 유지 (일부 차원 붕괴 가능) |
| **부정부호 (Indefinite)**                  | 양수와 음수 고유값 혼재 | 일부는 확장, 일부는 축소               |

### 준정부호(Semi-Definite)의 특별한 의미

적어도 하나의 고유값이 **0**인 경우:

- 행렬이 **준정부호(Semi-Definite)**
- **행렬식 = 0** (이전 영상에서 배운 내용!)
- **역행렬 존재하지 않음**
- 행렬을 곱하면 **최소 한 차원이 붕괴**

```python
# 준정부호 행렬 예시 (고유값 중 하나가 0)
A = np.array([[1, 2],
              [2, 4]])

lambdas, V = np.linalg.eig(A)
print(f"고유값: {lambdas}")      # [5. 0.] ← 0이 있음!
print(f"행렬식: {np.linalg.det(A)}")  # 0.0

# 역행렬 시도 → 에러!
# np.linalg.inv(A)  # LinAlgError: Singular matrix
```

### Goodfellow et al. (2016) 참조

더 자세한 내용은 **Deep Learning Book** (Goodfellow, Bengio, Courville, 2016)의 **2장**을 참조하세요.

---

## 3️⃣ 고유값 분해의 실제 적용 사례

### 3.1 고유얼굴 (Eigenfaces) - 얼굴 인식

**개념**: 얼굴 데이터셋의 **기본 구성 요소(고유벡터)**를 추출합니다.

- 수천 장의 얼굴 이미지로 공분산 행렬 생성
- 고유값 분해를 통해 **고유얼굴(Eigenfaces)** 추출
- 모든 얼굴은 고유얼굴의 **선형 결합**으로 표현 가능

$$
\text{얼굴} = a_1 \cdot \text{고유얼굴}_1 + a_2 \cdot \text{고유얼굴}_2 + \dots
$$

**활용**:

- 얼굴 인식 시스템
- 얼굴 압축 및 복원
- 새로운 얼굴 생성

### 3.2 고유목소리 (Eigenvoices) - 음성 인식

**개념**: 음성 데이터셋의 기본 목소리 패턴을 추출합니다.

- 다양한 화자의 음성 데이터 수집
- 고유값 분해로 **고유목소리(Eigenvoices)** 추출
- 새로운 화자의 목소리를 고유목소리의 조합으로 표현

**활용**:

- 화자 인식
- 음성 합성
- 적은 데이터로 새 화자 적응

### 3.3 고유진동수 (Eigenfrequencies) - 물리학/공학

**개념**: 구조물이나 시스템의 **고유 진동 모드**를 분석합니다.

**활용**:

- 건물/다리의 구조 안정성 분석
- 악기 설계 (고유 공명 주파수)
- 지진 공학

### 3.4 양자역학 (Quantum Mechanics)

**슈뢰딩거 파동 방정식**에서 고유값 분해는 핵심입니다:

- **고유값** = 에너지 준위 (측정 가능한 에너지 값)
- **고유벡터** = 파동 함수 (입자의 상태)

**활용**:

- 분자 궤도 계산
- 전자의 발생 확률 예측
- 화학 결합 분석

### 3.5 감염병 전파 모델 (R₀ 재생산 지수)

**Next Generation Matrix**의 최대 고유값이 바로 **기초 재생산 지수 R₀**입니다!

```
R₀ = 최대 고유값 (Next Generation Matrix)
```

- **R₀ > 1**: 감염병 확산
- **R₀ < 1**: 감염병 종식
- **R₀ = 1**: 풍토병화 (Endemic)

**활용**:

- 코로나19, 인플루엔자 등 감염병 모델링
- 백신 접종률 계산
- 방역 정책 수립

---

## 4️⃣ 머신러닝에서의 핵심 적용

### 4.1 특이값 분해 (SVD, Singular Value Decomposition)

고유값 분해의 **일반화** 버전으로, **직사각형 행렬**에도 적용 가능합니다.

$$
A = U \Sigma V^T
$$

**활용**:

- 이미지/동영상 압축
- 추천 시스템
- 노이즈 제거
- 잠재 의미 분석 (LSA)

→ **다음 장에서 자세히 학습!**

### 4.2 무어-펜로즈 역행렬 (Moore-Penrose Pseudoinverse)

역행렬이 존재하지 않는 행렬에 대한 **"최선의 근사 역행렬"**입니다.

$$
A^+ = V \Sigma^+ U^T
$$

**활용**:

- 과결정/과소결정 선형 시스템 해결
- 회귀 분석에서 최적의 가중치 찾기

→ **다음 장에서 자세히 학습!**

### 4.3 주성분 분석 (PCA, Principal Component Analysis)

**차원 축소**의 대표적인 알고리즘입니다.

**원리**:

1. 데이터의 공분산 행렬 계산
2. 고유값 분해 수행
3. 가장 큰 고유값에 해당하는 고유벡터 = **주성분(Principal Component)**
4. 주성분 방향으로 데이터 투영 → 차원 축소

**활용**:

- 고차원 데이터 시각화
- 노이즈 제거
- 특성 추출 (Feature Extraction)
- 연산 효율화

→ **SVD, Moore-Penrose 이후 학습!**

---

## 5️⃣ 코드로 보는 적용 사례: 간단한 PCA

```python
import numpy as np
import matplotlib.pyplot as plt

# 1. 2차원 데이터 생성 (상관관계 있음)
np.random.seed(42)
mean = [0, 0]
cov = [[2, 1.5], [1.5, 1]]  # 공분산 행렬
data = np.random.multivariate_normal(mean, cov, 100)

print(f"데이터 shape: {data.shape}")  # (100, 2)

# 2. 데이터 중심화 (평균을 0으로)
data_centered = data - np.mean(data, axis=0)

# 3. 공분산 행렬 계산
cov_matrix = np.cov(data_centered.T)
print(f"공분산 행렬:\n{cov_matrix}")

# 4. 고유값 분해
eigenvalues, eigenvectors = np.linalg.eig(cov_matrix)
print(f"\n고유값: {eigenvalues}")
print(f"고유벡터:\n{eigenvectors}")

# 5. 고유값 크기순 정렬
idx = np.argsort(eigenvalues)[::-1]  # 내림차순
eigenvalues = eigenvalues[idx]
eigenvectors = eigenvectors[:, idx]

print(f"\n정렬된 고유값: {eigenvalues}")
print(f"제1 주성분 (가장 큰 분산 방향): {eigenvectors[:, 0]}")
print(f"제2 주성분 (두 번째 분산 방향): {eigenvectors[:, 1]}")

# 6. 분산 설명 비율
variance_ratio = eigenvalues / np.sum(eigenvalues)
print(f"\n분산 설명 비율: {variance_ratio}")
print(f"제1 주성분이 설명하는 분산: {variance_ratio[0]*100:.1f}%")
```

**출력 예시**:

```
고유값: [2.85.. 0.75..]
분산 설명 비율: [0.79.. 0.21..]
제1 주성분이 설명하는 분산: 79.2%
```

**해석**: 제1 주성분만으로도 전체 분산의 약 80%를 설명할 수 있습니다!

---

## 🎯 요약: Linear Algebra II 파트 정리

### 이번 파트에서 배운 내용

| 주제                 | 핵심 내용                                           |
| :------------------- | :-------------------------------------------------- |
| **행렬 적용 (곱셈)** | 데이터에 행렬을 곱하면 변환이 일어남                |
| **아핀 변형**        | 스케일링, 회전, 밀림, 반사 등 2D 변환               |
| **고유벡터**         | 변환 후에도 방향이 유지되는 특수 벡터               |
| **고유값**           | 고유벡터가 스케일링되는 비율                        |
| **행렬식**           | 공간의 부피 변화율 = 모든 고유값의 곱               |
| **고유값 분해**      | $A = V \Lambda V^{-1}$로 행렬 분해                  |
| **적용 사례**        | 얼굴 인식, 음성 인식, 양자역학, 감염병 모델, PCA 등 |

### 다음 파트 예고: 머신러닝을 위한 행렬 연산

| 주제                   | 내용                                        |
| :--------------------- | :------------------------------------------ |
| **특이값 분해 (SVD)**  | 고유값 분해의 일반화, 모든 행렬에 적용 가능 |
| **무어-펜로즈 역행렬** | 역행렬이 없는 행렬에 대한 최선의 해         |
| **주성분 분석 (PCA)**  | 차원 축소, 데이터 압축, 시각화              |

---

## 다음 단계

**특이값 분해(SVD)**로 넘어갑니다. SVD는 고유값 분해를 **직사각형 행렬**에도 적용할 수 있게 일반화한 것으로, 머신러닝에서 가장 널리 사용되는 행렬 분해 기법입니다.
