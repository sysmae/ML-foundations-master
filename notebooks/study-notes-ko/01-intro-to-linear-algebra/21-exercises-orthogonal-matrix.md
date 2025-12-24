# 21. 연습문제: 직교 행렬 증명 📝

> Segment 3 마무리 - 진짜 직교 행렬인지 확인해보자!

## 🎯 목표

1. **단위 행렬($\mathbf{I}_3$)**이 직교 행렬임을 증명 (수동 계산 & 코드)
2. **복잡한 행렬($\mathbf{K}$)**가 직교 행렬임을 증명

---

## 📝 예제 1 & 2: 단위 행렬 ($\mathbf{I}_3$) 증명

$$
\mathbf{I}_3 = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

### 1단계: 열끼리 직교하는가? (Orthogonal)

모든 열 쌍의 **내적**이 0이어야 함.

- **Col 1 $\cdot$ Col 2**: $(1)(0) + (0)(1) + (0)(0) = 0$ ✅
- **Col 1 $\cdot$ Col 3**: $(1)(0) + (0)(0) + (0)(1) = 0$ ✅
- **Col 2 $\cdot$ Col 3**: $(0)(0) + (1)(0) + (0)(1) = 0$ ✅

### 2단계: 단위 노름을 가지는가? (Unit Norm)

모든 열의 **$L^2$ 노름**이 1이어야 함.

- **Col 1**: $\sqrt{1^2 + 0^2 + 0^2} = 1$ ✅
- **Col 2**: $\sqrt{0^2 + 1^2 + 0^2} = 1$ ✅
- **Col 3**: $\sqrt{0^2 + 0^2 + 1^2} = 1$ ✅

### 결론

모든 열이 **정규직교벡터(Orthonormal Vector)**이므로, **직교 행렬**입니다!

---

## 📝 예제 3: 단위 행렬 코드 증명 (NumPy)

```python
import numpy as np

I = np.eye(3)

# 1. 직교성 확인 (Col 1과 Col 2 내적)
print(np.dot(I[:,0], I[:,1]))  # 0.0

# 2. 단위 노름 확인 (Col 1의 노름)
print(np.linalg.norm(I[:,0]))  # 1.0
```

---

## 📝 예제 4: 행렬 $\mathbf{K}$ 증명

보기엔 복잡해 보이는 행렬 $\mathbf{K}$가 직교 행렬인지 확인해 봅시다.

$$
\mathbf{K} = \frac{1}{3} \begin{bmatrix} 2 & 1 & 2 \\ -2 & 2 & 1 \\ 1 & 2 & -2 \end{bmatrix} = \begin{bmatrix} 2/3 & 1/3 & 2/3 \\ -2/3 & 2/3 & 1/3 \\ 1/3 & 2/3 & -2/3 \end{bmatrix}
$$

### 1단계: 수동 계산 (직교성)

**Col 1 $\cdot$ Col 2**:

$$
\left(\frac{2}{3}\right)\left(\frac{1}{3}\right) + \left(-\frac{2}{3}\right)\left(\frac{2}{3}\right) + \left(\frac{1}{3}\right)\left(\frac{2}{3}\right)
$$

$$
= \frac{2}{9} - \frac{4}{9} + \frac{2}{9} = \frac{0}{9} = 0 \quad ✅
$$

_(마찬가지로 Col 1-3, Col 2-3도 0이 나옵니다)_

### 2단계: 수동 계산 (단위 노름)

**Col 1의 길이**:

$$
\sqrt{\left(\frac{2}{3}\right)^2 + \left(-\frac{2}{3}\right)^2 + \left(\frac{1}{3}\right)^2}
$$

$$
= \sqrt{\frac{4}{9} + \frac{4}{9} + \frac{1}{9}} = \sqrt{\frac{9}{9}} = \sqrt{1} = 1 \quad ✅
$$

---

### 3단계: PyTorch 코드로 증명 (가장 확실한 방법!)

**핵심 특성** $\mathbf{K}^T \mathbf{K} = \mathbf{I}$ 를 이용해 한 번에 증명!

```python
import torch

K = torch.tensor([
    [2/3, 1/3, 2/3],
    [-2/3, 2/3, 1/3],
    [1/3, 2/3, -2/3]
])

# K 전치 * K 계산
result = torch.matmul(K.T, K)

print(result)
# tensor([[ 1.0000e+00, -2.9802e-08,  0.0000e+00],
#         [-2.9802e-08,  1.0000e+00, -1.4901e-08],
#         [ 0.0000e+00, -1.4901e-08,  1.0000e+00]])
# (아주 작은 오차를 무시하면 단위 행렬 I와 같습니다!)
```

### ✅ 최종 결론

행렬 $\mathbf{K}$는 **직교 행렬**입니다.
따라서 $\mathbf{K}^{-1} = \mathbf{K}^T$ 가 성립합니다. (복잡한 역행렬 계산 필요 없음!)

---

## 🎉 Segment 3 종료

이것으로 선형대수학 기초의 **행렬 특성** 파트를 모두 마쳤습니다.
고생하셨습니다! 👏👏👏
