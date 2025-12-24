# 9. 텐서 축소 (Tensor Reduction)

> Segment 2: 기본 텐서 연산의 일부

## 📌 개요

텐서 축소(Reduction)는 텐서의 **여러 요소를 하나의 값으로 줄이는** 연산입니다.  
가장 대표적인 예가 **합계(sum)** 연산입니다.

---

## 📐 수학적 정의

### 벡터 합계

n개 요소를 가진 벡터 $\mathbf{x}$의 합:

$$
\sum_{i=1}^{n} x_i = x_1 + x_2 + \cdots + x_n
$$

### 행렬 합계

m행 n열 행렬 $\mathbf{X}$의 합:

$$
\sum_{i=1}^{m} \sum_{j=1}^{n} X_{ij}
$$

모든 요소를 더합니다!

---

## 💻 전체 합계 (Total Sum)

### 예제 행렬

```python
import numpy as np

X = np.array([[25,  2],
              [ 5, 26],
              [ 3,  7]])
# 6개 요소: 25 + 2 + 5 + 26 + 3 + 7 = 68
```

### NumPy

```python
X.sum()  # 68
```

### PyTorch

```python
import torch

X_pt = torch.tensor([[25, 2], [5, 26], [3, 7]])
torch.sum(X_pt)  # tensor(68)
```

### TensorFlow

```python
import tensorflow as tf

X_tf = tf.constant([[25, 2], [5, 26], [3, 7]])
tf.reduce_sum(X_tf)  # <tf.Tensor: ... numpy=68>
```

---

## 📊 축(axis)별 축소

### 핵심 개념

행렬이나 고차원 텐서에서는 **특정 축만 따라 축소**할 수 있습니다!

```
X (3×2):
┌─────────┐
│ 25 │  2 │  → axis=1: 행별 합
├─────────┤
│  5 │ 26 │
├─────────┤
│  3 │  7 │
└─────────┘
    ↓
 axis=0: 열별 합
```

### axis=0 (열별 합, 세로 방향)

```python
np.sum(X, axis=0)  # [33, 35]
```

계산:

- 첫 번째 열: `25 + 5 + 3 = 33`
- 두 번째 열: `2 + 26 + 7 = 35`

### axis=1 (행별 합, 가로 방향)

```python
np.sum(X, axis=1)  # [27, 31, 10]
```

계산:

- 첫 번째 행: `25 + 2 = 27`
- 두 번째 행: `5 + 26 = 31`
- 세 번째 행: `3 + 7 = 10`

---

## 💻 라이브러리별 축 지정

### NumPy

```python
np.sum(X, axis=0)  # 열별 합: [33, 35]
np.sum(X, axis=1)  # 행별 합: [27, 31, 10]
```

### PyTorch

```python
torch.sum(X_pt, dim=0)  # 열별: tensor([33, 35])
torch.sum(X_pt, dim=1)  # 행별: tensor([27, 31, 10])
```

> PyTorch는 `axis` 대신 `dim` 사용!

### TensorFlow

```python
tf.reduce_sum(X_tf, axis=0)  # 열별: [33, 35]
tf.reduce_sum(X_tf, axis=1)  # 행별: [27, 31, 10]
```

---

## 🔄 다른 축소 연산들

합계 외에도 다양한 축소 연산이 있습니다:

| 연산 | NumPy       | PyTorch        | TensorFlow         |
| ---- | ----------- | -------------- | ------------------ |
| 합계 | `np.sum()`  | `torch.sum()`  | `tf.reduce_sum()`  |
| 최대 | `np.max()`  | `torch.max()`  | `tf.reduce_max()`  |
| 최소 | `np.min()`  | `torch.min()`  | `tf.reduce_min()`  |
| 평균 | `np.mean()` | `torch.mean()` | `tf.reduce_mean()` |
| 곱   | `np.prod()` | `torch.prod()` | `tf.reduce_prod()` |

```python
# NumPy 예제
np.max(X)       # 26 (전체 최대값)
np.min(X)       # 2 (전체 최소값)
np.mean(X)      # 11.33... (평균)
np.max(X, axis=0)  # [25, 26] (열별 최대)
```

---

## 💡 핵심 정리

1. **축소 = 여러 요소 → 하나의 값**
2. **전체 축소**: 인자 없이 사용 → 스칼라 결과
3. **축별 축소**: `axis` (또는 `dim`) 지정 → 저차원 텐서 결과
4. **axis=0**: 세로 방향 (열별)
5. **axis=1**: 가로 방향 (행별)
6. 합계가 ML에서 **가장 자주 사용**되는 축소 연산!

---

## 🎯 다음 주제

텐서 축소를 이해했다면, 다음으로 **내적(Dot Product)** 연산을 배워봅시다!
