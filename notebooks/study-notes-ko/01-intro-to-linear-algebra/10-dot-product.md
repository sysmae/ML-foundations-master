# 10. 내적 (Dot Product) ⭐

> Segment 2: 기본 텐서 연산의 일부

## 📌 개요

**내적(Dot Product)**은 딥러닝에서 **가장 흔하게 사용되는 텐서 연산** 중 하나입니다.  
심층 인공신경망의 **모든 인공 뉴런**에서 이 연산이 수행됩니다!

---

## 📐 수학적 정의

### 전제 조건

두 벡터 $\mathbf{x}$와 $\mathbf{y}$가 있을 때:

- **반드시 같은 크기(n)** 여야 함!

### 표기법

내적을 표현하는 여러 방법:

| 표기                                     | 설명                  |
| ---------------------------------------- | --------------------- |
| $\mathbf{x} \cdot \mathbf{y}$            | 점 표기 (가장 일반적) |
| $\mathbf{x}^T \mathbf{y}$                | 전치 행렬 곱 표기     |
| $\langle \mathbf{x}, \mathbf{y} \rangle$ | 꺾쇠 표기             |

### 계산 공식

$$
\mathbf{x} \cdot \mathbf{y} = \sum_{i=1}^{n} x_i y_i = x_1 y_1 + x_2 y_2 + \cdots + x_n y_n
$$

**2단계 과정**:

1. **요소별 곱셈** (아다마르 곱)
2. **모든 결과를 합산** (축소)

> 결과는 항상 **스칼라(단일 값)**!

---

## 🔍 단계별 예제

### 벡터 정의

```python
import numpy as np

x = np.array([25, 2, 5])
y = np.array([0, 1, 2])
```

### Step 1: 요소별 곱셈

```python
# 각 위치의 요소끼리 곱함
element_wise = x * y
# [25*0, 2*1, 5*2] = [0, 2, 10]
```

### Step 2: 합산 (축소)

```python
dot_product = np.sum(element_wise)
# 0 + 2 + 10 = 12
```

### 시각적 표현

```
x:        [25,  2,  5]
          × × ×
y:        [ 0,  1,  2]
          ─────────────
곱셈:      [ 0,  2, 10]
          ─────────────
합:              12     ← 스칼라!
```

---

## 💻 라이브러리별 구현

### NumPy

```python
import numpy as np

x = np.array([25, 2, 5])
y = np.array([0, 1, 2])

# np.dot() 메서드 사용
result = np.dot(x, y)  # 12
```

### PyTorch

```python
import torch

x_pt = torch.tensor([25., 2., 5.])  # float 타입!
y_pt = torch.tensor([0., 1., 2.])   # float 타입!

# torch.dot() 메서드 사용
result = torch.dot(x_pt, y_pt)  # tensor(12.)
```

> ⚠️ **주의**: PyTorch의 `torch.dot()`은 **float 타입 텐서**가 필요합니다!  
> 정수 텐서가 아닌 `[25., 2., 5.]`처럼 소수점을 붙여주세요.

### TensorFlow

TensorFlow에서는 **2단계**로 수행:

```python
import tensorflow as tf

x_tf = tf.constant([25, 2, 5])
y_tf = tf.constant([0, 1, 2])

# Step 1: 요소별 곱셈
product = tf.multiply(x_tf, y_tf)  # [0, 2, 10]

# Step 2: 합산
result = tf.reduce_sum(product)  # 12
```

> TensorFlow는 NumPy/PyTorch보다 코드가 길지만, 내부 동작이 더 명시적입니다.

---

## 🧠 딥러닝에서의 중요성

### 인공 뉴런의 핵심 연산

```
입력들:     [x₁, x₂, x₃, ..., xₙ]
가중치:     [w₁, w₂, w₃, ..., wₙ]
                    ↓
              내적 계산
                    ↓
출력:       Σ(xᵢ × wᵢ) + bias
```

- 수백만 개의 뉴런이 이 연산을 수행
- 학습 과정에서 **수억 번** 반복
- 따라서 내적은 ML에서 가장 핵심적인 연산!

---

## 📊 라이브러리 비교 요약

| 라이브러리 | 메서드                              | 특징            |
| ---------- | ----------------------------------- | --------------- |
| NumPy      | `np.dot(x, y)`                      | 간단하고 직관적 |
| PyTorch    | `torch.dot(x, y)`                   | float 타입 필요 |
| TensorFlow | `tf.multiply()` + `tf.reduce_sum()` | 2단계, 명시적   |

---

## 💡 핵심 정리

1. **내적 = 요소별 곱셈 + 합산**
2. **조건**: 두 벡터의 크기가 **반드시 동일**
3. **결과**: 항상 **스칼라** (단일 값)
4. **딥러닝 핵심**: 모든 인공 뉴런에서 사용
5. NumPy/PyTorch: 한 줄 코드, TensorFlow: 2단계

---

## 🎯 다음 주제

내적을 이해했다면, 이제 선형 연립방정식을 풀어보는 실습을 해봅시다!
