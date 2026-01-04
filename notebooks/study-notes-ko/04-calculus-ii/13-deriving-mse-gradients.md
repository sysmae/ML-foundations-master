# 13. MSE 기울기 수기 유도 (Deriving MSE Gradients)

> **"배치 회귀의 핵심: MSE 미분하기"**

이제 단일 데이터(Squred Error)가 아닌, 전체 데이터의 **평균 제곱 오차(MSE)**에 대한 기울기를 손으로 직접 유도해 봅시다.
($\sum$ 기호가 들어가지만 겁먹지 마세요!)

---

## 1️⃣ 함수 정의

### 비용 함수 (Cost Function): MSE

$$ C = \frac{1}{n} \sum (u_i)^2 $$
(여기서 $u_i = \hat{y}_i - y_i$)

### 예측 함수 (Prediction)

$$ \hat{y}\_i = m x_i + b $$

---

## 2️⃣ 연쇄 법칙 적용 (Step-by-Step)

### Step A: 비용 $C$를 $u$로 미분 ($\frac{\partial C}{\partial u}$)

상수인 $\frac{1}{n}$과 $\sum$은 그대로 두고, 제곱 항만 미분합니다.
$$ \frac{\partial C}{\partial u} = \frac{1}{n} \sum 2u = \frac{2}{n} \sum (\hat{y} - y) $$

### Step B: $u$를 $m, b$로 미분

이전과 동일합니다.

- $\frac{\partial u}{\partial \hat{y}} = 1$
- $\frac{\partial \hat{y}}{\partial m} = x$
- $\frac{\partial \hat{y}}{\partial b} = 1$

### Step C: 최종 결합

1. **$\frac{\partial C}{\partial m}$** (기울기 $m$에 대한 변화율):
   $$ \frac{\partial C}{\partial m} = \frac{\partial C}{\partial u} \cdot \frac{\partial u}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial m} $$
   $$ = \left( \frac{2}{n} \sum (\hat{y} - y) \right) \cdot 1 \cdot x $$
   **$$ \therefore \frac{\partial C}{\partial m} = \frac{2}{n} \sum x(\hat{y} - y) $$\*\*

2. **$\frac{\partial C}{\partial b}$** ($y$절편 $b$에 대한 변화율):
   $$ \frac{\partial C}{\partial b} = \frac{\partial C}{\partial u} \cdot \frac{\partial u}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial b} $$
   $$ = \left( \frac{2}{n} \sum (\hat{y} - y) \right) \cdot 1 \cdot 1 $$
   **$$ \therefore \frac{\partial C}{\partial b} = \frac{2}{n} \sum (\hat{y} - y) $$\*\*

---

## 3️⃣ Python 코드로 검증

위에서 유도한 공식을 코드로 짜서, PyTorch의 `backward()` 결과와 일치하는지 확인해 봅시다.

```python
import torch

# 데이터 (8개)
x = torch.tensor([0., 1., 2., 3., 4., 5., 6., 7.])
y = torch.tensor([1.86, 1.31, .62, .33, .09, -.67, -1.23, -1.37])
m = torch.tensor(0.9, requires_grad=True)
b = torch.tensor(0.1, requires_grad=True)
n = len(x)

# Forward
y_hat = m*x + b
diff = y_hat - y

# 1. 수기 계산 (Manual Calculation)
grad_m_manual = (2/n) * torch.sum(x * diff)
grad_b_manual = (2/n) * torch.sum(diff)

print(f"Manual dC/dm: {grad_m_manual.item():.3f}")  # 36.305
print(f"Manual dC/db: {grad_b_manual.item():.3f}")  # 6.265

# 2. PyTorch 자동 미분 (Autodiff)
mse = torch.sum(diff**2) / n
mse.backward()

print(f"Autodiff dC/dm: {m.grad.item():.3f}")       # 36.305
print(f"Autodiff dC/db: {b.grad.item():.3f}")       # 6.265
```

**결과: 완벽하게 일치합니다!** 🎉

---

## 🎯 결론

우리는 이제 블랙박스를 열고 그 안을 들여다보았습니다.

- **자동 미분**은 마법이 아니라, 우리가 유도한 이 **편미분 공식(`2/n * sum...`)을 아주 빠르게 수행하는 계산기**일 뿐입니다.
- 이 원리를 알면, 나중에 모델이 학습되지 않을 때 "왜 기울기가 0이 되었을까?(Vanishing Gradient)" 같은 문제를 수학적으로 분석할 수 있는 힘이 생깁니다.

이것으로 **Calculus II: Partial Derivatives & Integrals**의 핵심인 **편미분과 경사 하강법** 파트를 마칩니다.
다음 주제는 **적분(Integrals)**입니다. 준비되셨나요? 🚀
