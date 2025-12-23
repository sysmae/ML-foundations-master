# 5. 텐서 연산

## 🔄 전치 (Transposition)

```python
import numpy as np

X = np.array([[1, 2], [3, 4], [5, 6]])
print(X.shape)    # (3, 2)
print(X.T.shape)  # (2, 3)
```

---

## ➕ 기본 산술

### 스칼라 연산 (브로드캐스팅)

```python
X = np.array([[1, 2], [3, 4]])

print(X + 10)   # 모든 요소에 +10
print(X * 2)    # 모든 요소에 ×2
```

### 요소별 연산 (Hadamard Product)

같은 shape끼리:

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(A + B)  # [[6, 8], [10, 12]]
print(A * B)  # [[5, 12], [21, 32]] ← 요소별 곱!
```

> ⚠️ `A * B`는 행렬 곱셈이 아님!

---

## 📉 축소 연산 (Reduction)

```python
X = np.array([[1, 2], [3, 4]])

# 전체
print(np.sum(X))      # 10
print(np.max(X))      # 4
print(np.mean(X))     # 2.5

# 축 지정
print(np.sum(X, axis=0))  # [4, 6] (열별 합)
print(np.sum(X, axis=1))  # [3, 7] (행별 합)
```

---

## 🎯 내적 (Dot Product) ⭐

### 벡터 내적

$$
\mathbf{a} \cdot \mathbf{b} = \sum_i a_i b_i
$$

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

dot = np.dot(a, b)  # 1*4 + 2*5 + 3*6 = 32
```

### 내적의 의미

- 두 벡터의 **유사도**
- 결과는 **스칼라**
- **신경망 뉴런**의 핵심 연산!

---

## ✖️ 행렬-벡터 곱

```python
A = np.array([[1, 2], [3, 4], [5, 6]])  # (3, 2)
x = np.array([10, 20])                   # (2,)

result = np.dot(A, x)  # (3,)
print(result)  # [50, 110, 170]
```

---

## ✖️ 행렬-행렬 곱

$$
\mathbf{C} = \mathbf{A} \mathbf{B}
$$

차원 규칙: `(m, n) @ (n, p) → (m, p)`

```python
A = np.array([[1, 2], [3, 4]])      # (2, 2)
B = np.array([[5, 6], [7, 8]])      # (2, 2)

C = np.dot(A, B)  # 또는 A @ B
print(C)  # [[19, 22], [43, 50]]
```

> ⚠️ 행렬 곱은 **교환법칙 ❌**: `AB ≠ BA`
