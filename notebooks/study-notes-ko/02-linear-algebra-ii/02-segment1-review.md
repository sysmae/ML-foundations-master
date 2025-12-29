# 2. 기초 선형대수 복습 (Review of Introductory Linear Algebra)

> Segment 1: 기초를 탄탄하게

이 섹션에서는 이전 과정에서 다루었던 선형대수학의 기초 개념들을 빠르게 복습하고, Python 코드(NumPy, PyTorch)를 통해 실습합니다.

---

## 1️⃣ 벡터 전치 (Vector Transposition)

벡터를 전치(Transpose)한다는 것은 행 벡터를 열 벡터로, 또는 그 반대로 바꾸는 것을 의미합니다.

### NumPy 예제

```python
import numpy as np

# 1차원 배열 생성
x = np.array([25, 2, 5])
print(x) # [25  2  5]

# 2차원 배열(행 벡터)로 생성
x = np.array([[25, 2, 5]])
print(x.shape) # (1, 3)

# 전치 (Transpose)
print(x.T)
# [[25]
#  [ 2]
#  [ 5]]
```

### PyTorch 예제

```python
import torch

x_p = torch.tensor([25, 2, 5])

# view 메서드를 사용하여 형상 변경 (메모리 변경 없이 모양만 바꿈)
print(x_p.view(3, 1))
```

---

## 2️⃣ $L^2$ 노름 ($L^2$ Norm)

$L^2$ 노름은 벡터의 "길이"나 "크기"를 나타냅니다. 유클리드 거리(Euclidean Distance)와 같습니다.

$$
\|x\|_2 = \sqrt{\sum_{i} x_i^2}
$$

### 코드 예제

```python
# NumPy
norm_np = np.linalg.norm(x)
print(norm_np) # 25.5734...

# 직접 계산
calc_norm = (25**2 + 2**2 + 5**2)**(1/2)
print(calc_norm) # 25.5734...

# PyTorch (float 타입이어야 함)
norm_torch = torch.norm(torch.tensor([25, 2, 5.]))
print(norm_torch)
```

---

## 3️⃣ 행렬 곱셈 (Matrix Multiplication)

**주의**: `*` 연산자는 **아다마르 곱(Hadamard Product)**으로, 같은 위치의 원소끼리 곱하는 연산입니다. 우리가 흔히 말하는 행렬 곱셈과는 다릅니다.

### 아다마르 곱 (Element-wise Multiplication)

$$
A \odot B
$$

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[2, 2], [2, 2]])

print(A * B)
# [[ 2  4]
#  [ 6  8]]
```

### 행렬 곱셈 (Matrix Multiplication)

$$
C = AB
$$

```python
# NumPy: np.dot 사용
print(np.dot(A, B))
# [[ 6  6]
#  [14 14]]

# PyTorch: torch.matmul 사용
A_p = torch.tensor([[1, 2], [3, 4]])
B_p = torch.tensor([[2, 2], [2, 2]])
print(torch.matmul(A_p, B_p))
```

---

## 4️⃣ 역행렬 (Matrix Inversion)과 선형 방정식 해결

역행렬 $A^{-1}$은 $A$와 곱했을 때 단위 행렬 $I$가 되는 행렬입니다. 이를 이용해 선형 방정식 $Ax = b$의 해($x$)를 구할 수 있습니다.

$$
x = A^{-1}b
$$

### 코드 예제

```python
# 1. 역행렬 구하기
X = np.array([[4, 2], [-5, -3]])
X_inv = np.linalg.inv(X)

print(X_inv)
# [[ 1.5  1. ]
#  [-2.5 -2. ]]

# 검증: X * X_inv = I
print(np.dot(X, X_inv))
# [[1. 0.]
#  [0. 1.]]

# 2. 선형 방정식 풀기
y = np.array([4, -7])
w = np.dot(X_inv, y)

print(w) # [-1.  4.] - 이것이 구하고자 하는 해입니다.
```

> **참고**: 역행렬은 존재하지 않을 수도 있습니다 (특이 행렬인 경우). 또한 수치적으로 불안정할 수 있어, 실제로는 가우스 소거법 등을 내부적으로 사용하는 `np.linalg.solve` 등을 더 많이 사용합니다.

---

## 🎯 요약

- **전치**: 행 ↔ 열 변환
- **$L^2$ 노름**: 벡터의 길이
- **행렬 곱**: `np.dot` (NumPy), `torch.matmul` (PyTorch) 사용 (`*`는 원소별 곱)
- **역행렬**: `np.linalg.inv`로 구하며, 선형 방정식을 푸는 데 사용됨
