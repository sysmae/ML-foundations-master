# 15. 프로베니우스 노름 (Frobenius Norm)

> Segment 3: 행렬 특성 (Matrix Properties) - 첫 번째 주제

## 📌 Segment 3 개요

이번 섹션에서 배울 내용:

- **프로베니우스 노름** ← 현재
- 행렬-벡터 곱셈
- 행렬-행렬 곱셈
- 대칭 행렬 & 단위 행렬
- **역행렬** (매우 중요!)
- 대각 행렬 & 직교 행렬

---

## 🔢 프로베니우스 노름이란?

**행렬의 크기(규모)를 측정**하는 함수입니다.

벡터의 $L^2$ 노름(유클리드 노름)을 **행렬로 확장**한 개념!

---

## 📐 수학적 정의

$$
\| \mathbf{X} \|_F = \sqrt{\sum_{i} \sum_{j} x_{ij}^2}
$$

### 표기법

- $\| \mathbf{X} \|_F$ : 행렬 X의 프로베니우스 노름
- 아래첨자 **F** = Frobenius

### 계산 과정

1. 행렬의 **모든 요소를 제곱**
2. 제곱한 값을 **모두 더함**
3. 합계의 **제곱근**

---

## 🔍 예제

### 행렬 정의

$$
\mathbf{X} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}
$$

### 수동 계산

$$
\| \mathbf{X} \|_F = \sqrt{1^2 + 2^2 + 3^2 + 4^2}
$$

$$
= \sqrt{1 + 4 + 9 + 16}
$$

$$
= \sqrt{30} \approx 5.48
$$

---

## 💻 라이브러리별 구현

### NumPy

```python
import numpy as np

X = np.array([[1, 2], [3, 4]])

# 수동 계산
manual = np.sqrt(np.sum(X**2))
print(manual)  # 5.477...

# np.linalg.norm 사용 (권장)
frob_norm = np.linalg.norm(X)
print(frob_norm)  # 5.477...
```

### PyTorch

```python
import torch

X_pt = torch.tensor([[1., 2.], [3., 4.]])  # float 타입!

frob_norm = torch.norm(X_pt)
print(frob_norm)  # tensor(5.4772)
```

> ⚠️ **주의**: PyTorch는 **float 타입** 텐서 필요!

### TensorFlow

```python
import tensorflow as tf

X_tf = tf.constant([[1., 2.], [3., 4.]])  # float 타입!

frob_norm = tf.norm(X_tf)
print(frob_norm)  # tf.Tensor(5.477..., shape=(), dtype=float32)
```

---

## 🔗 L² 노름과의 관계

| 개념              | 적용 대상 | 의미                        |
| ----------------- | --------- | --------------------------- |
| $L^2$ 노름        | 벡터      | 벡터의 길이 (유클리드 거리) |
| 프로베니우스 노름 | 행렬      | 행렬의 크기                 |

**공통점**: 둘 다 **유클리드 거리** 개념!

### 또 다른 해석

프로베니우스 노름 = 행렬 내 **모든 열 벡터의 규모를 합산**한 값

---

## 💡 핵심 정리

1. 프로베니우스 노름 = **행렬 크기 측정**
2. 계산: 모든 요소 **제곱 → 합산 → 제곱근**
3. 벡터 $L^2$ 노름의 **행렬 버전**
4. 표기: $\| \mathbf{X} \|_F$
5. 코드: `np.linalg.norm(X)`, `torch.norm(X)`, `tf.norm(X)`

---

## 🎯 다음 주제

프로베니우스 노름을 이해했다면, 다음으로 **행렬 곱셈 (Matrix Multiplication)** 을 배워봅시다!
