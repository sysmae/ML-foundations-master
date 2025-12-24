# 19. 역행렬 (Matrix Inversion) ⭐

> Segment 3: 행렬 특성 - ML에서 매우 강력한 도구!

## 📌 개요

**역행렬**은 선형 방정식을 **컴퓨터로 풀어내는** 똑똑하고 편리한 방법입니다.

- 치환법/소거법: 손으로 푸는 방법
- **역행렬**: 컴퓨터로 푸는 방법

---

## 📐 역행렬의 정의

### 표기법

$$
\mathbf{X}^{-1}
$$

### 핵심 특성

$$
\mathbf{X}^{-1} \mathbf{X} = \mathbf{I}
$$

> 역행렬과 원래 행렬을 곱하면 → **단위 행렬**!

---

## 🧠 회귀 문제에서의 활용

### 회귀 방정식

$$
\mathbf{y} = \mathbf{Xw}
$$

| 기호         | 의미               | 예시                  |
| ------------ | ------------------ | --------------------- |
| $\mathbf{y}$ | 결과값 (예측 대상) | 집 가격               |
| $\mathbf{X}$ | 특성 행렬          | 침실 수, 학교 거리 등 |
| $\mathbf{w}$ | 가중치 (미지수)    | 학습할 매개변수       |

### 미지수 w 풀기

**목표**: $\mathbf{w}$를 구하고 싶다!

**풀이**:

$$
\mathbf{Xw} = \mathbf{y}
$$

양변에 $\mathbf{X}^{-1}$ 곱하기:

$$
\mathbf{X}^{-1}\mathbf{Xw} = \mathbf{X}^{-1}\mathbf{y}
$$

$$
\mathbf{Iw} = \mathbf{X}^{-1}\mathbf{y}
$$

$$
\boxed{\mathbf{w} = \mathbf{X}^{-1}\mathbf{y}}
$$

---

## 🔍 예제

### 문제

연립방정식:

$$
\begin{cases}
4b + 2c = 4 \\
-5b - 3c = -7
\end{cases}
$$

### 텐서로 표현

$$
\mathbf{X} = \begin{bmatrix} 4 & 2 \\ -5 & -3 \end{bmatrix}, \quad
\mathbf{y} = \begin{bmatrix} 4 \\ -7 \end{bmatrix}, \quad
\mathbf{w} = \begin{bmatrix} b \\ c \end{bmatrix}
$$

### 풀이

$$
\mathbf{w} = \mathbf{X}^{-1}\mathbf{y}
$$

---

## 💻 코드 구현

### NumPy

```python
import numpy as np

X = np.array([[4, 2], [-5, -3]])
y = np.array([4, -7])

# 역행렬 계산
X_inv = np.linalg.inv(X)
print(X_inv)
# [[ 1.5  1. ]
#  [-2.5 -2. ]]

# w 구하기
w = np.dot(X_inv, y)
print(w)  # [-1.  4.]  → b = -1, c = 4

# 검증: Xw = y
print(np.dot(X, w))  # [ 4. -7.]  ✓
```

### PyTorch

```python
import torch

X = torch.tensor([[4., 2.], [-5., -3.]])
y = torch.tensor([4., -7.])

X_inv = torch.linalg.inv(X)
w = torch.matmul(X_inv, y)
print(w)  # tensor([-1.,  4.])
```

### TensorFlow

```python
import tensorflow as tf

X = tf.constant([[4., 2.], [-5., -3.]])
y = tf.constant([4., -7.])

X_inv = tf.linalg.inv(X)
w = tf.linalg.matvec(X_inv, y)
```

---

## ⚠️ 역행렬의 한계

### 1. 특이 행렬 (Singular Matrix)

**모든 열이 선형 독립**이어야 역행렬 존재!

| 문제 상황   | 예시                                       | 결과    |
| ----------- | ------------------------------------------ | ------- |
| 평행한 열   | $\begin{bmatrix}1 & 2\\2 & 4\end{bmatrix}$ | 해 없음 |
| 일치하는 열 | $\begin{bmatrix}1 & 1\\2 & 2\end{bmatrix}$ | 무한 해 |

```python
# 특이 행렬 → 오류 발생!
X = np.array([[1, 2], [2, 4]])
np.linalg.inv(X)  # LinAlgError: Singular matrix
```

### 2. 정방 행렬 (Square Matrix) 필요

역행렬은 **행 = 열**인 경우에만 존재!

| 유형       | 조건    | 문제           |
| ---------- | ------- | -------------- |
| 과결정     | 행 > 열 | 교차점 여러 개 |
| 불충분결정 | 행 < 열 | 직선이 부족    |

---

## 💡 핵심 정리

1. **역행렬 정의**: $\mathbf{X}^{-1}\mathbf{X} = \mathbf{I}$
2. **활용**: $\mathbf{w} = \mathbf{X}^{-1}\mathbf{y}$ 로 미지수 풀기
3. **코드**: `np.linalg.inv(X)`, `torch.linalg.inv(X)`, `tf.linalg.inv(X)`
4. **제한 조건**:
   - 정방 행렬 (행 = 열)
   - 특이 행렬 아님 (열이 선형 독립)

---

## 🎯 다음 주제

역행렬을 이해했다면, 다음으로 **대각 행렬 (Diagonal Matrix)** 과 **직교 행렬 (Orthogonal Matrix)** 을 배워봅시다!
