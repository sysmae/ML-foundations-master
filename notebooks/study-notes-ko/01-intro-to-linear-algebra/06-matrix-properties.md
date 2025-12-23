# 6. 행렬 성질

## 📏 프로베니우스 노름

행렬 버전의 L² 노름:

$$
\|\mathbf{A}\|_F = \sqrt{\sum_i \sum_j A_{ij}^2}
$$

```python
import numpy as np

A = np.array([[1, 2], [3, 4]])
frobenius = np.linalg.norm(A)  # 5.477
```

---

## 🔲 특수 행렬

### 대칭 행렬

$\mathbf{X}^T = \mathbf{X}$

```python
X = np.array([[1, 2, 3],
              [2, 4, 5],
              [3, 5, 6]])
print(np.array_equal(X, X.T))  # True
```

### 단위 행렬 (Identity)

대각선 1, 나머지 0. 곱셈의 항등원!

$$
\mathbf{I} \mathbf{X} = \mathbf{X} \mathbf{I} = \mathbf{X}
$$

```python
I = np.eye(3)
X = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(np.dot(I, X))  # X 그대로
```

---

## 🔃 역행렬 (Inverse)

$\mathbf{X}^{-1}$이 존재하면:

$$
\mathbf{X}^{-1} \mathbf{X} = \mathbf{X} \mathbf{X}^{-1} = \mathbf{I}
$$

### 조건

- **정사각 행렬**이어야 함
- **비특이(non-singular)**여야 함 (행렬식 ≠ 0)

```python
X = np.array([[4, 7], [2, 6]])
X_inv = np.linalg.inv(X)

# 검증
print(np.dot(X, X_inv))  # [[1, 0], [0, 1]] (단위 행렬)
```

### 역행렬이 없는 경우

```python
# 특이 행렬 (singular)
singular = np.array([[1, 2], [2, 4]])
# np.linalg.inv(singular)  # LinAlgError!
```

---

## ⊥ 직교 행렬(Orthogonal)

열벡터들이 서로 직교 + 단위 노름:

$$
\mathbf{Q}^T \mathbf{Q} = \mathbf{I}
$$

**특성**: $\mathbf{Q}^{-1} = \mathbf{Q}^T$ (역행렬 = 전치!)

```python
# 단위 행렬은 직교 행렬
I = np.eye(3)
print(np.dot(I.T, I))  # I
```

---

## ⊥ 직교 벡터

내적이 0:

$$
\mathbf{a} \perp \mathbf{b} \iff \mathbf{a} \cdot \mathbf{b} = 0
$$

```python
a = np.array([1, 0])  # x축
b = np.array([0, 1])  # y축
print(np.dot(a, b))   # 0 → 직교!
```

---

## 📝 핵심 요약

| 개념              | 설명          |
| ----------------- | ------------- |
| 프로베니우스 노름 | 행렬의 크기   |
| 대칭 행렬         | $X^T = X$     |
| 단위 행렬         | 곱셈의 항등원 |
| 역행렬            | $X^{-1}X = I$ |
| 직교 행렬         | $Q^TQ = I$    |
| 직교 벡터         | 내적 = 0      |
