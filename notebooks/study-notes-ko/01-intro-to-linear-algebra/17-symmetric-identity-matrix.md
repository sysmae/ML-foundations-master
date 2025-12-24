# 17. 대칭 행렬과 단위 행렬 (Symmetric & Identity Matrix)

> Segment 3: 행렬 특성 - 특별한 형태의 행렬들

## 📌 개요

특별한 특성을 가진 두 가지 행렬 유형:

1. **대칭 행렬 (Symmetric Matrix)**
2. **단위 행렬 (Identity Matrix)** - 대칭 행렬의 특수 케이스

---

## 🔄 대칭 행렬 (Symmetric Matrix)

### 정의

1. **정사각형** 행렬 (행 = 열)
2. **전치 = 자기 자신**

$$
\mathbf{X}^T = \mathbf{X}
$$

### 예제

$$
\mathbf{X} = \begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 5 \\ 3 & 5 & 6 \end{bmatrix}
$$

**관찰:**

- 대각선 기준 **위아래가 거울 대칭**
- 대각선 요소(1, 4, 6)는 자유롭게 설정 가능

### 코드 확인

```python
import numpy as np

X = np.array([[1, 2, 3],
              [2, 4, 5],
              [3, 5, 6]])

# 전치와 원본 비교
print(X.T)
# [[1 2 3]
#  [2 4 5]
#  [3 5 6]]

# 동일한지 확인
print(np.array_equal(X, X.T))  # True
```

---

## 🎯 단위 행렬 (Identity Matrix) ⭐

### 정의

대칭 행렬의 **특수 케이스**:

1. **대각선 요소 = 1**
2. **나머지 요소 = 0**

### 표기법

$$
\mathbf{I}_n
$$

- $n$ = 행렬의 크기 (n×n)

### 예제

$$
\mathbf{I}_4 = \begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

---

## ✨ 단위 행렬의 특별한 성질

### 핵심 특성

**어떤 벡터(또는 행렬)에 단위 행렬을 곱하면 → 그대로!**

$$
\mathbf{I}_n \cdot \mathbf{x} = \mathbf{x}
$$

> 숫자에서 **1을 곱하는 것**과 같은 효과!

### 코드 예제

```python
import torch

# 3×3 단위 행렬 생성
I = torch.eye(3)
print(I)
# tensor([[1., 0., 0.],
#         [0., 1., 0.],
#         [0., 0., 1.]])

# 벡터 정의
x = torch.tensor([25., 2., 5.])

# 단위 행렬 × 벡터 = 원래 벡터
result = torch.matmul(I, x)
print(result)  # tensor([25., 2., 5.])
```

---

## 💻 라이브러리별 단위 행렬 생성

### NumPy

```python
import numpy as np

I = np.eye(3)  # 3×3 단위 행렬
```

### PyTorch

```python
import torch

I = torch.eye(3)  # 3×3 단위 행렬
```

### TensorFlow

```python
import tensorflow as tf

I = tf.eye(3)  # 3×3 단위 행렬
```

---

## 🤔 왜 함수 이름이 `eye`일까?

**말장난(pun)** 입니다! 😄

| 표기    | 발음   | 의미                            |
| ------- | ------ | ------------------------------- |
| **I**   | "아이" | **I**dentity matrix (단위 행렬) |
| **eye** | "아이" | 영어 발음이 같음                |

```
Identity → I → eye
단위 행렬 → 아이 → eye()
```

> 💡 MATLAB에서 처음 이 이름을 사용했고, NumPy가 따라했습니다!

---

## 📊 요약 비교

| 특성      | 대칭 행렬 | 단위 행렬  |
| --------- | --------- | ---------- |
| 형태      | 정사각형  | 정사각형   |
| 대칭성    | $X^T = X$ | $I^T = I$  |
| 대각선    | 임의의 값 | **1만**    |
| 비대각선  | 대칭 값   | **0만**    |
| 곱셈 효과 | 다양함    | **항등원** |

---

## 💡 핵심 정리

1. **대칭 행렬**: 정사각형 + 전치 = 자기 자신
2. **단위 행렬**: 대각선 1, 나머지 0
3. 단위 행렬 곱셈: $\mathbf{Ix} = \mathbf{x}$ (숫자의 1과 같은 역할)
4. 표기: $\mathbf{I}_n$ (n×n 크기)
5. 생성: `np.eye(n)`, `torch.eye(n)`, `tf.eye(n)`

---

## 🎯 다음 주제

대칭/단위 행렬을 이해했다면, 다음으로 **역행렬 (Matrix Inversion)** 을 배워봅시다!
