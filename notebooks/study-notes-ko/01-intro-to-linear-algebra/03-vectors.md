# 3. 벡터 (Vectors)

## 📌 정의

**벡터** = 수의 1차원 배열 = **Rank 1 텐서**

---

## ✏️ 표기법

| 표기  | 의미       | 스타일     |
| ----- | ---------- | ---------- |
| **x** | 벡터 전체  | **볼드체** |
| $x_i$ | i번째 요소 | _이탤릭체_ |

---

## 🗺️ 벡터 = 공간의 좌표 + 규모/방향

$$
\mathbf{x} = \begin{bmatrix} 12 \\ 4 \end{bmatrix}
$$

- **좌표 해석**: 2D 평면의 점 (12, 4)
- **벡터 해석**: 원점(0,0)에서 (12,4)까지의 화살표

---

## 🐍 NumPy로 생성

```python
import numpy as np

x = np.array([25, 2, 5])
print(len(x))     # 3
print(x.shape)    # (3,)
print(x[0])       # 25 (0-indexed!)
```

---

## 🔄 벡터 전치

```python
# ❌ 1D 배열은 전치 안됨
x = np.array([25, 2, 5])
print(x.T.shape)  # (3,) ← 변화 없음!

# ✅ 2D로 만들어야 전치 작동
y = np.array([[25, 2, 5]])  # 이중 대괄호
print(y.T.shape)   # (3, 1)
```

---

## 📏 노름 (Norm) — 벡터의 크기

| 노름       | 공식                | [25,2,5] 결과 | ML 활용       |
| ---------- | ------------------- | ------------- | ------------- |
| **L²**     | $\sqrt{\sum x_i^2}$ | 25.57         | 유클리드 거리 |
| **L¹**     | $\sum\|x_i\|$       | 32            | Lasso, 희소성 |
| **제곱L²** | $\sum x_i^2$        | 654           | 저비용 연산   |
| **Max**    | $\max\|x_i\|$       | 25            | 최대 오차     |

```python
x = np.array([25, 2, 5])
np.linalg.norm(x)      # L² 노름: 25.57
np.sum(np.abs(x))      # L¹ 노름: 32
np.dot(x, x)           # 제곱 L²: 654
```

---

## ⭐ 단위벡터 (Unit Vector)

L² 노름이 **1**인 벡터:

```python
x = np.array([3, 4])
unit_x = x / np.linalg.norm(x)
print(unit_x)  # [0.6, 0.8]
```

---

# 🧱 기저벡터 (Basis Vector) — 공간의 건축가

## 📌 정의

**기저벡터** = 벡터 공간의 모든 벡터를 표현하기 위한 **최소한의 기준 벡터 집합**

> "공간이라는 건물을 짓기 위한 가장 기본적인 벽돌"

## 핵심 조건

1. **선형 결합**: 기저를 늘리고(스칼라배) 더하면 공간의 모든 벡터 생성 가능
2. **선형 독립**: 기저 벡터끼리는 서로를 만들어낼 수 없음

## 표준 기저 (Standard Basis)

2차원 공간 $\mathbb{R}^2$:

$$
\mathbf{i} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \quad
\mathbf{j} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

## 벡터 분해 예시

$$
\mathbf{v} = \begin{bmatrix} 1.5 \\ 2 \end{bmatrix} = 1.5 \cdot \mathbf{i} + 2 \cdot \mathbf{j}
$$

> 💡 좌표값 (x, y)는 사실 **기저 벡터의 계수(Coefficient)**들의 목록!

---

# ⊥ 직교벡터 (Orthogonal Vector)

## 📌 정의

두 벡터가 **90도(직각)**를 이루는 상태

$$
\mathbf{x} \perp \mathbf{y} \iff \mathbf{x} \cdot \mathbf{y} = 0
$$

## 내적으로 판별

```python
x = np.array([3, 0])
y = np.array([0, 2])
print(np.dot(x, y))  # 0 → 직교!
```

## 의미

- **독립성**: 서로 영향을 주지 않음
- **상관관계 없음**: x축으로 이동해도 y축 좌표는 불변
- **ML에서**: 독립적인 특징(Feature) 추출

## n차원 법칙

> "n차원 공간에서는 서로 직교하는 벡터가 **최대 n개** 존재한다."

- 2D: 최대 2개 (x축, y축)
- 3D: 최대 3개 (x, y, z축)

---

# ✨ 정규직교벡터 (Orthonormal Vector)

## 📌 정의

**직교** + **정규(단위)** = 정규직교

1. **Orthogonal**: 서로 수직 (내적 = 0)
2. **Normal**: 크기 = 1 (L² 노름 = 1)

$$
\text{길이가 1이면서 서로 90도를 이루는 벡터들}
$$

## 검증 예시: 표준 기저

```python
i = np.array([1, 0])
j = np.array([0, 1])

# 직교 확인
print(np.dot(i, j))       # 0 ✓

# 단위 크기 확인
print(np.linalg.norm(i))  # 1 ✓
print(np.linalg.norm(j))  # 1 ✓
```

→ **i, j는 정규직교벡터!**

## 왜 중요한가? (ML 핵심!)

| 장점              | 설명                        |
| ----------------- | --------------------------- |
| **계산 단순화**   | 자기 내적 = 1, 다른 것 = 0  |
| **수치적 안정성** | Overflow/Underflow 방지     |
| **역행렬 = 전치** | $A^{-1} = A^T$ (계산 이득!) |

---

## 📊 세 개념 비교

| 구분     | 기저벡터            | 직교벡터              | 정규직교벡터         |
| -------- | ------------------- | --------------------- | -------------------- |
| **정의** | 공간 생성 최소 집합 | 내적 = 0              | 내적=0 AND 크기=1    |
| **핵심** | 선형 독립           | 각도 90°              | 각도 90° + 크기 1    |
| **예시** | [1,0], [1,1]        | [3,0], [0,2]          | [1,0], [0,1]         |
| **관계** | 모든 공간의 필수    | 기저가 직교일 필요 ❌ | **가장 이상적 기저** |

---

## 🔥 PyTorch / TensorFlow

```python
# PyTorch
import torch
x_pt = torch.tensor([25.0, 2.0, 5.0])

# TensorFlow
import tensorflow as tf
x_tf = tf.Variable([25.0, 2.0, 5.0])
```
