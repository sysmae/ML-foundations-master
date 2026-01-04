# 10. 단일 데이터 회귀 모델의 기울기 (Single Point Regression Gradient)

> **"자동 미분 없이, 손으로 직접 Gradient 구해보기 (준비편)"**

지금까지는 PyTorch의 자동 미분(`backward()`)이 마법처럼 기울기를 구해줬지만, 이제는 **"도대체 내부에서 어떤 계산이 일어나는가?"**를 파헤칠 시간입니다.
가장 간단한 모델인 **단일 데이터 포인트 회귀 (Single Point Regression)**를 통해 이 과정을 준비해 봅시다.

---

## 1️⃣ 시나리오 설정

복잡함을 피하기 위해 딱 **하나의 데이터 포인트**만 사용합니다.

- **데이터**: $x = 7.0$ (약물 투여량), $y = -1.37$ (건망증 수치)
- **모델**: $y = mx + b$
- **파라미터 초기값**: $m = 0.9, b = 0.1$ (임의의 값)

---

## 2️⃣ Forward Pass (순전파)

입력값($x$)을 넣어서 예측값($\hat{y}$)을 구합니다.

$$ \hat{y} = mx + b = 0.9(7.0) + 0.1 = 6.3 + 0.1 = \mathbf{6.4} $$

실제 값($-1.37$)과는 차이가 큽니다.

---

## 3️⃣ Calculate Cost (비용 계산)

데이터가 하나뿐이므로 **평균**을 낼 필요 없이, 단순 **제곱 오차(Quadratic Cost)**를 사용합니다.

$$ C = (\hat{y} - y)^2 $$
$$ C = (6.4 - (-1.37))^2 = (7.77)^2 \approx \mathbf{60.37} $$

비용이 매우 큽니다. 이제 이 비용을 줄이기 위해 $m$과 $b$를 어떻게 조절해야 할까요?

---

## 4️⃣ PyTorch로 정답 미리보기

우리가 손으로 계산할 값이 맞는지 확인하기 위해, 먼저 PyTorch에게 답을 물어봅시다.

```python
import torch

# 1. 데이터 및 파라미터 설정
x = torch.tensor(7.)
y = torch.tensor(-1.37)
m = torch.tensor(0.9, requires_grad=True)
b = torch.tensor(0.1, requires_grad=True)

# 2. Forward Pass
y_hat = m*x + b

# 3. Calculate Cost
squared_error = (y_hat - y)**2
print(f"Cost: {squared_error.item():.2f}") # 약 60.37

# 4. Backward Pass (자동 미분)
squared_error.backward()

# 정답 공개
print(f"dC/dm: {m.grad.item():.2f}") # 약 108.78
print(f"dC/db: {b.grad.item():.2f}") # 약 15.54
```

기계가 계산한 기울기는 다음과 같습니다.

- $\frac{\partial C}{\partial m} \approx 109$
- $\frac{\partial C}{\partial b} \approx 15.5$

다음 장에서는 **편미분과 연쇄 법칙**을 사용하여, 이 숫자들이 어떻게 나왔는지 **종이와 펜으로 직접 유도**해 보겠습니다.
