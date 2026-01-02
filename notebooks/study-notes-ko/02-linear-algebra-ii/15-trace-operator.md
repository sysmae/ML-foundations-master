# 15. 대각합 연산자 (Trace Operator)

> **"대각선 요소들의 합 — 가장 단순하지만 강력한 도구"**

이번 장에서는 선형대수학의 마지막 개념인 **대각합(Trace)**을 배웁니다. 개념 자체는 매우 단순하지만, 머신러닝의 이론적 토대(특히 PCA, 노름 계산 등)에서 자주 등장하는 중요한 연산자입니다.

---

## 1️⃣ 대각합(Trace)이란?

### 정의

정방행렬 $A$의 **대각합**은 $\text{Tr}(A)$로 표기하며, **주대각선 요소들의 합**입니다.

$$
\text{Tr}(A) = \sum_{i} A_{i,i}
$$

### NumPy로 계산하기

```python
import numpy as np

# 행렬 A 정의
A = np.array([[25, 2],
              [5, 4]])

# 대각합 계산: 25 + 4 = 29
trace_A = np.trace(A)

print(f"행렬 A:\n{A}")
print(f"대각합 Tr(A): {trace_A}")
```

---

## 2️⃣ 대각합의 주요 성질

### 1. 전치해도 같다

행렬을 전치하더라도 주대각선 요소들은 자리가 바뀌지 않습니다. 따라서 대각합은 변하지 않습니다.

$$
\text{Tr}(A) = \text{Tr}(A^T)
$$

```python
# 검증
print(f"Tr(A): {np.trace(A)}")      # 29
print(f"Tr(A.T): {np.trace(A.T)}")  # 29
# 결과: 동일함
```

### 2. 순환성 (Cyclic Property)

여러 행렬의 곱에 대한 대각합은 **순서가 순환해도** 값이 같습니다. (차원이 맞아야 함)

$$
\text{Tr}(ABC) = \text{Tr}(BCA) = \text{Tr}(CAB)
$$

**주의**: $\text{Tr}(ABC) \neq \text{Tr}(BAC)$ (순서가 뒤섞이면 다름!)

```python
# 예제 행렬 정의
A = np.array([[1, 2], [3, 4]])
B = np.array([[-1, 0], [1, 2]])
C = np.array([[0, 1], [1, 0]])

# 곱셈 결과
ABC = A @ B @ C
BCA = B @ C @ A
CAB = C @ A @ B

# 대각합 비교
print(f"Tr(ABC): {np.trace(ABC)}")
print(f"Tr(BCA): {np.trace(BCA)}")
print(f"Tr(CAB): {np.trace(CAB)}")
# 결과: 세 값이 모두 동일함
```

---

## 3️⃣ 프로베니우스 노름 (Frobenius Norm)과의 관계

**프로베니우스 노름**은 행렬의 모든 요소의 제곱합의 제곱근으로, 벡터의 L2 노름을 행렬로 확장한 개념입니다. (선형대수학 입문에서 배웠죠!)

이 노름은 **대각합을 이용해** 우아하게 계산할 수 있습니다:

$$
\|A\|_F = \sqrt{\text{Tr}(AA^T)}
$$

### 코드 증명

```python
# 1. linalg.norm으로 직접 계산
frobenius_norm = np.linalg.norm(A, 'fro')
print(f"np.linalg.norm: {frobenius_norm}")

# 2. 대각합으로 계산: sqrt(Tr(A A^T))
trace_method = np.sqrt(np.trace(A @ A.T))
print(f"sqrt(Tr(AA^T)): {trace_method}")

# 결과: 두 값이 동일함!
```

---

## 4️⃣ PyTorch 연습 문제

### 문제 1: 대각합 기본 연산 구현

파이토치를 사용하여 행렬 $A_P$의 대각합을 구하고, 전치행렬의 대각합과 같음을 증명하세요.

```python
import torch

# 행렬 정의
A_p = torch.tensor([[25., 2.],
                    [5., 4.]])

# TODO: 대각합 계산 (torch.trace 사용)

# TODO: 전치행렬의 대각합 계산 및 비교
```

### 문제 2: 프로베니우스 노름 공식 검증

파이토치로 $\|A\|_F = \sqrt{\text{Tr}(AA^T)}$ 공식이 성립함을 검증하세요.

```python
# TODO: torch.norm(A_p) 계산

# TODO: torch.sqrt(torch.trace(torch.matmul(A_p, A_p.T))) 계산

# TODO: 두 값 비교
# (참고) A_p @ A_p.T 로 행렬 곱 가능
```

### 힌트

- PyTorch 대각합: `torch.trace(A)`
- PyTorch 노름: `torch.norm(A)` 또는 `torch.linalg.norm(A)`
- 행렬 곱: `torch.matmul(A, B)` 또는 `A @ B`

---

## 🎯 요약

### 핵심 개념

1. **대각합** = $\sum A_{i,i}$ (주대각선 합)
2. **전치 불변**: $\text{Tr}(A) = \text{Tr}(A^T)$
3. **순환성**: $\text{Tr}(ABC) = \text{Tr}(BCA) = \text{Tr}(CAB)$
4. **프로베니우스 노름**: $\|A\|_F = \sqrt{\text{Tr}(AA^T)}$

### 머신러닝에서의 중요성

- **PCA (주성분 분석)**: 대각합 연산자가 핵심적으로 사용됨
- **비용 함수**: 행렬 미분 과정에서 대각합의 성질이 활용됨
- **정규화**: 가중치 행렬의 크기를 제한할 때 프로베니우스 노름 사용

---

## 🏁 선형대수학 II 이론 완료!

축하합니다! 이것으로 **선형대수학 II: 행렬 연산**의 모든 이론 학습을 마쳤습니다.

이제 마지막 단계로, 지금까지 배운 모든 지식(SVD, 대각합, 고유값 등)을 총동원하여 머신러닝의 강력한 알고리즘인 **주성분 분석(PCA)**을 정복하러 떠납니다!
