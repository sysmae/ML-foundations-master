# 16. 행렬 곱셈 (Matrix Multiplication) ⭐

> Segment 3: 행렬 특성 - ML에서 가장 중요한 연산!

## 📌 개요

행렬 곱셈은 **머신러닝에서 가장 중요하고 널리 쓰이는 수학 연산**입니다!

> ⚠️ 아다마르 곱(요소별 곱셈)과는 완전히 다른 연산!

---

## 🔑 행렬 곱셈의 조건

$$
\mathbf{A}_{m \times n} \cdot \mathbf{B}_{n \times p} = \mathbf{C}_{m \times p}
$$

### 핵심 규칙

**첫 번째 행렬의 열 = 두 번째 행렬의 행** 이어야 함!

```
A (m × n)  ×  B (n × p)  =  C (m × p)
      ↑         ↑
      └────┬────┘
         같아야 함!
```

### 결과 행렬의 크기

- **행**: 첫 번째 행렬(A)의 행 수 (m)
- **열**: 두 번째 행렬(B)의 열 수 (p)

---

## 📐 수학적 공식

$$
C_{ik} = \sum_{j=1}^{n} A_{ij} \cdot B_{jk}
$$

**해석**:

- $C$의 i행 k열 요소
- = A의 i행과 B의 k열의 **내적**

---

## 🔍 예제 1: 행렬 × 벡터

### 문제

$$
\mathbf{A} = \begin{bmatrix} 3 & 4 \\ 5 & 6 \\ 7 & 8 \end{bmatrix}, \quad
\mathbf{b} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}
$$

### 조건 확인

- A: 3×**2**, b: **2**×1 → ✓ 곱셈 가능!
- 결과: 3×1

### 계산

$$
\begin{bmatrix}
3 \cdot 1 + 4 \cdot 2 \\
5 \cdot 1 + 6 \cdot 2 \\
7 \cdot 1 + 8 \cdot 2
\end{bmatrix}
= \begin{bmatrix} 3 + 8 \\ 5 + 12 \\ 7 + 16 \end{bmatrix}
= \begin{bmatrix} 11 \\ 17 \\ 23 \end{bmatrix}
$$

---

## 🔍 예제 2: 행렬 × 행렬

### 문제

$$
\mathbf{A} = \begin{bmatrix} 3 & 4 \\ 5 & 6 \\ 7 & 8 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} 1 & 9 \\ 2 & 0 \end{bmatrix}
$$

### 조건 확인

- A: 3×**2**, B: **2**×2 → ✓ 곱셈 가능!
- 결과: 3×2

### 계산

**첫 번째 열** (B의 [1,2] 열 사용):

$$
\begin{bmatrix} 3 \cdot 1 + 4 \cdot 2 \\ 5 \cdot 1 + 6 \cdot 2 \\ 7 \cdot 1 + 8 \cdot 2 \end{bmatrix} = \begin{bmatrix} 11 \\ 17 \\ 23 \end{bmatrix}
$$

**두 번째 열** (B의 [9,0] 열 사용):

$$
\begin{bmatrix} 3 \cdot 9 + 4 \cdot 0 \\ 5 \cdot 9 + 6 \cdot 0 \\ 7 \cdot 9 + 8 \cdot 0 \end{bmatrix} = \begin{bmatrix} 27 \\ 45 \\ 63 \end{bmatrix}
$$

### 결과

$$
\mathbf{C} = \begin{bmatrix} 11 & 27 \\ 17 & 45 \\ 23 & 63 \end{bmatrix}
$$

---

## 💻 라이브러리별 구현

### NumPy

```python
import numpy as np

A = np.array([[3, 4], [5, 6], [7, 8]])
B = np.array([[1, 9], [2, 0]])

C = np.dot(A, B)  # 또는 A @ B
print(C)
# [[11 27]
#  [17 45]
#  [23 63]]
```

### PyTorch

```python
import torch

A_pt = torch.tensor([[3, 4], [5, 6], [7, 8]])
B_pt = torch.tensor([[1, 9], [2, 0]])

C_pt = torch.matmul(A_pt, B_pt)  # 또는 A_pt @ B_pt
```

### TensorFlow

```python
import tensorflow as tf

A_tf = tf.constant([[3, 4], [5, 6], [7, 8]])
B_tf = tf.constant([[1, 9], [2, 0]])

# 행렬-벡터: tf.linalg.matvec()
# 행렬-행렬: tf.linalg.matmul()
C_tf = tf.linalg.matmul(A_tf, B_tf)
```

---

## ⚠️ 중요: 행렬 곱셈은 비가환적!

$$
\mathbf{AB} \neq \mathbf{BA}
$$

```python
# A @ B ≠ B @ A (대부분의 경우)
# 심지어 B @ A가 불가능할 수도 있음! (차원 불일치)
```

---

## 🧠 ML에서의 활용

### 회귀 모델

$$
\mathbf{y} = \mathbf{Xw}
$$

- $\mathbf{X}$: 특성 행렬 (n개 샘플 × m개 특성)
- $\mathbf{w}$: 가중치 벡터 (m × 1)
- $\mathbf{y}$: 예측값 벡터 (n × 1)

### 딥러닝 (신경망)

$$
\mathbf{z} = \mathbf{Xw} + \mathbf{b}
$$

- 입력 × 가중치 행렬 + 편향
- 모든 레이어에서 행렬 곱셈 수행!

---

## 📊 요약 비교

| 구분 | 아다마르 곱      | 행렬 곱셈           |
| ---- | ---------------- | ------------------- |
| 기호 | `*` 또는 $\odot$ | `@` 또는 `np.dot()` |
| 조건 | 같은 shape       | 내부 차원 일치      |
| 결과 | shape 유지       | shape 변환          |
| 연산 | 요소별 곱        | 행×열 내적의 합     |

---

## 💡 핵심 정리

1. 조건: **A의 열 = B의 행**
2. 결과 크기: (A의 행) × (B의 열)
3. 각 요소 = **행과 열의 내적**
4. **비가환적**: AB ≠ BA
5. ML의 핵심: 회귀, 신경망 등 모든 곳에서 사용!

---

## 🎯 다음 주제

행렬 곱셈을 이해했다면, 다음으로 **단위 행렬 (Identity Matrix)** 과 **대칭 행렬 (Symmetric Matrix)** 을 배워봅시다!
