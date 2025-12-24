# 20. 대각 행렬과 직교 행렬 (Diagonal & Orthogonal Matrix)

> Segment 3: 행렬 특성 - 마지막 주제! 🎉

## 📌 개요

선형대수학 소개의 마지막 두 가지 특별 행렬:

1. **대각 행렬 (Diagonal Matrix)**
2. **직교 행렬 (Orthogonal Matrix)**

---

# Part 1: 대각 행렬 (Diagonal Matrix)

## 정의

**주대각선 외의 모든 요소가 0**인 행렬

$$
\mathbf{D} = \begin{bmatrix} d_1 & 0 & 0 \\ 0 & d_2 & 0 \\ 0 & 0 & d_3 \end{bmatrix}
$$

> 💡 **단위 행렬**은 대각선이 모두 1인 대각 행렬의 특수 케이스!

## 표기법

$$
\text{diag}(\mathbf{x})
$$

- $\mathbf{x}$: 대각선 요소들의 벡터

## 연산 효율성 ⚡

### 곱셈

$$
\text{diag}(\mathbf{x}) \cdot \mathbf{y} = \mathbf{x} \odot \mathbf{y}
$$

대각 행렬 × 벡터 = **아다마르 곱**과 동일! (연산 효율적)

### 역행렬

$$
\text{diag}(\mathbf{x})^{-1} = \text{diag}\left(\frac{1}{x_1}, \frac{1}{x_2}, \ldots, \frac{1}{x_n}\right)
$$

각 대각선 요소의 **역수**를 취하면 끝!

> ⚠️ 대각선에 **0이 있으면** 역행렬 불가능!

---

# Part 2: 직교 행렬 (Orthogonal Matrix)

## 정의

**모든 행과 열이 정규직교벡터**로 이루어진 행렬

### 정규직교벡터가 되려면?

1. **길이가 1** (단위 노름)
2. **서로 수직** (내적 = 0)

---

## 📌 직교 행렬인지 확인하는 법 (완전 상세 예시)

### Step 1: 직교 행렬 준비

아무 숫자나 넣으면 직교 행렬이 안 됩니다!  
**"길이가 1이고 서로 수직"** 조건을 만족해야 합니다.

> 💡 피타고라스 정리 3:4:5 비율 → 0.6과 0.8 사용

$$
\mathbf{A} = \begin{bmatrix} 0.6 & -0.8 \\ 0.8 & 0.6 \end{bmatrix}
$$

### Step 2: 조건 검사

**조건 1 - 길이가 1인가?**

첫 번째 열의 길이:

$$
\sqrt{0.6^2 + 0.8^2} = \sqrt{0.36 + 0.64} = \sqrt{1} = 1 \quad ✓
$$

**조건 2 - 서로 수직인가?**

두 열의 내적:

$$
(0.6 \times -0.8) + (0.8 \times 0.6) = -0.48 + 0.48 = 0 \quad ✓
$$

> ✅ **A는 완벽한 직교 행렬입니다!**

---

## 핵심 특성

$$
\mathbf{A}^T \mathbf{A} = \mathbf{A} \mathbf{A}^T = \mathbf{I}
$$

> 전치와 곱하면 → **단위 행렬**!

---

## 📐 직접 계산해보기: $\mathbf{A}^T \times \mathbf{A} = \mathbf{I}$

### Step 1: 전치 행렬 만들기

행과 열을 바꿉니다:

$$
\mathbf{A}^T = \begin{bmatrix} 0.6 & 0.8 \\ -0.8 & 0.6 \end{bmatrix}
$$

### Step 2: 곱셈 계산

$$
\begin{bmatrix} 0.6 & 0.8 \\ -0.8 & 0.6 \end{bmatrix} \times \begin{bmatrix} 0.6 & -0.8 \\ 0.8 & 0.6 \end{bmatrix}
$$

**(1,1) 위치**: $(0.6 \times 0.6) + (0.8 \times 0.8) = 0.36 + 0.64 = \mathbf{1}$

**(1,2) 위치**: $(0.6 \times -0.8) + (0.8 \times 0.6) = -0.48 + 0.48 = \mathbf{0}$

**(2,1) 위치**: $(-0.8 \times 0.6) + (0.6 \times 0.8) = -0.48 + 0.48 = \mathbf{0}$

**(2,2) 위치**: $(-0.8 \times -0.8) + (0.6 \times 0.6) = 0.64 + 0.36 = \mathbf{1}$

### Step 3: 결과 확인

$$
\mathbf{A}^T \mathbf{A} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} = \mathbf{I} \quad ✓
$$

> 🎯 **전치와 곱했더니 단위 행렬이 나왔습니다!**

---

## 💡 그래서 왜 전치 = 역행렬?

**역행렬의 정의**: _"나랑 곱해서 I(단위 행렬) 나오면 넌 내 역행렬이야"_

$$
\mathbf{A}^{-1} \mathbf{A} = \mathbf{I}
$$

**방금 계산한 것**: _"A에다가 $\mathbf{A}^T$를 곱했더니 I가 나왔네?"_

$$
\mathbf{A}^T \mathbf{A} = \mathbf{I}
$$

**결론**: _"$\mathbf{A}^T$가 곧 $\mathbf{A}^{-1}$이구나!"_

$$
\boxed{\mathbf{A}^{-1} = \mathbf{A}^T}
$$

> ✨ 이래서 직교 행렬에서만 **"뒤집으면 곧 역행렬"** 공식이 성립!

## 🌟 가장 중요한 특성

$$
\boxed{\mathbf{A}^{-1} = \mathbf{A}^T}
$$

**역행렬 = 전치**

### 이게 무슨 뜻일까? 🤔

**원래 역행렬의 정의**를 떠올려 봅시다:

$$
\mathbf{A}^{-1} \mathbf{A} = \mathbf{I}
$$

**직교 행렬의 특성**에서:

$$
\mathbf{A}^T \mathbf{A} = \mathbf{I}
$$

두 식을 비교하면:

- 왼쪽 식: $\mathbf{A}^{-1}$을 곱하면 $\mathbf{I}$
- 오른쪽 식: $\mathbf{A}^T$을 곱하면 $\mathbf{I}$

**둘 다 같은 결과** → 따라서 $\mathbf{A}^{-1} = \mathbf{A}^T$ !

### 수학적 유도

$$
\mathbf{A}^T \mathbf{A} = \mathbf{I}
$$

양변에 $\mathbf{A}^{-1}$ 곱하기:

$$
\mathbf{A}^T \mathbf{A} \mathbf{A}^{-1} = \mathbf{I} \mathbf{A}^{-1}
$$

$$
\mathbf{A}^T (\mathbf{A} \mathbf{A}^{-1}) = \mathbf{A}^{-1}
$$

$$
\mathbf{A}^T \mathbf{I} = \mathbf{A}^{-1}
$$

$$
\mathbf{A}^T = \mathbf{A}^{-1}
$$

### 왜 중요한가?

| 연산        | 효율성               | 설명                  |
| ----------- | -------------------- | --------------------- |
| 역행렬 계산 | 매우 **비효율적** ❌ | 복잡한 수학 연산 필요 |
| 전치 계산   | 매우 **효율적** ✅   | 행과 열만 바꾸면 끝!  |

### 📌 실제 예시로 이해하기

---

#### 🔴 일반 행렬의 경우 (전치 ≠ 역행렬)

$$
\mathbf{A} = \begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix}
$$

**전치**:

$$
\mathbf{A}^T = \begin{bmatrix} 2 & 1 \\ 3 & 4 \end{bmatrix}
$$

**역행렬** (복잡한 계산 필요):

$$
\mathbf{A}^{-1} = \frac{1}{\det(\mathbf{A})} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix} = \frac{1}{5} \begin{bmatrix} 4 & -3 \\ -1 & 2 \end{bmatrix} = \begin{bmatrix} 0.8 & -0.6 \\ -0.2 & 0.4 \end{bmatrix}
$$

> ❌ **전치 ≠ 역행렬** → 일반 행렬은 복잡한 역행렬 계산이 필요!

---

#### 🟢 직교 행렬의 경우 (전치 = 역행렬!)

직교 행렬 예시 (90도 회전 행렬):

$$
\mathbf{Q} = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
$$

**전치**:

$$
\mathbf{Q}^T = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}
$$

**역행렬** (공식으로 계산):

$$
\mathbf{Q}^{-1} = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}
$$

> ✅ **전치 = 역행렬!** 복잡한 계산 없이 행과 열만 바꾸면 됨!

**검증**: $\mathbf{Q}^T \mathbf{Q} = \mathbf{I}$

$$
\begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix} \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} = \mathbf{I} \quad ✓
$$

---

#### 🎯 결론

| 행렬 종류     | 역행렬 구하는 방법                 |
| ------------- | ---------------------------------- |
| 일반 행렬     | 복잡한 공식 (행렬식, 여인수 등)    |
| **직교 행렬** | **그냥 전치!** 행과 열만 바꾸면 끝 |

```python
# 일반 행렬: 복잡한 계산 필요
A_inv = np.linalg.inv(A)  # 내부적으로 복잡한 연산 수행

# 직교 행렬: 전치만 하면 됨!
Q_inv = Q.T  # 행과 열만 바꿈 - 초간단!
```

직교 행렬에서는 역행렬이 필요할 때 **전치만 하면 끝**!

---

## 💻 코드 예제

```python
import numpy as np

# 대각 행렬 생성
D = np.diag([2, 3, 4])
print(D)
# [[2 0 0]
#  [0 3 0]
#  [0 0 4]]

# 대각 행렬의 역행렬
D_inv = np.diag([1/2, 1/3, 1/4])
print(np.dot(D, D_inv))  # 단위 행렬
```

---

## 📊 특별 행렬 정리표

| 행렬 유형 | 조건                | 특성             |
| --------- | ------------------- | ---------------- |
| **대칭**  | $X^T = X$           | 대각선 기준 대칭 |
| **단위**  | 대각선 1, 나머지 0  | $IX = X$         |
| **대각**  | 대각선 외 0         | 연산 효율적      |
| **직교**  | 정규직교벡터로 구성 | $A^{-1} = A^T$   |

---

## 🎉 선형대수학 소개 완료!

### Segment 3: 행렬 특성 요약

| 주제              | 핵심 내용      |
| ----------------- | -------------- |
| 프로베니우스 노름 | 행렬 크기 측정 |
| 행렬 곱셈         | ML의 핵심 연산 |
| 대칭/단위 행렬    | $IX = X$       |
| 역행렬            | $w = X^{-1}y$  |
| 대각/직교 행렬    | 연산 효율성    |

---

## 🚀 다음 단계

**선형대수학 II**에서 배울 내용:

- 고유벡터 & 고유값
- 행렬 분해
- 특이값 분해 (SVD)
- 무어-펜로즈 유사역행렬

---

## 🎓 축하합니다!

**ML Foundations** 시리즈의 첫 번째 주제  
**"선형대수학 소개"**를 완료했습니다! 🎊
