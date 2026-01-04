# 12. 경사 하강법과 배치 회귀 (Gradient Descent & Batch Regression)

> **"눈 가린 삼엽충이 산을 내려가는 법"**

Gradient($\nabla C$)는 **비용이 가장 빠르게 증가하는 방향**을 가리킨다고 했습니다.
그렇다면 비용을 줄이려면? 당연히 그 **반대 방향**으로 가야겠죠. 이것이 바로 **경사 하강법(Gradient Descent)**입니다.

---

## 1️⃣ 경사 하강법의 직관적 이해

상상해 봅시다.

- 여러분은 앞이 보이지 않는 **삼엽충(Trilobite)**입니다. 😎
- 산(비용 곡선, Cost Curve) 어딘가에 서 있습니다.
- 목표: 산의 가장 낮은 곳(Minimum)인 '집'으로 가야 합니다.

어떻게 해야 할까요?

1. 지팡이로 주변을 툭툭 쳐봅니다 (현재 위치의 **기울기/Gradient** 계산).
2. 경사가 아래로 내려가는 방향을 찾습니다 (Gradient의 **반대 방향**).
3. 그쪽으로 한 걸음 내딛습니다 (파라미터 **업데이트**).
4. 도착할 때까지 반복합니다.

이것이 머신러닝의 4단계 루프 중 **Step 4: Optimization**입니다.

---

## 2️⃣ 배치 회귀 (Batch Regression)로 확장

이전에는 데이터 하나(Single Point)만 썼지만, 실제로는 **모든 데이터(Batch)**를 한꺼번에 사용해야 효율적입니다.
8개의 데이터를 모두 사용하는 **배치 회귀**를 셋팅해 봅시다.

### Step 1: Forward Pass (Batch)

- 입력 $x$는 이제 8개의 숫자가 담긴 벡터입니다.
- $y = mx + b$ 연산도 벡터 연산으로 한 번에 처리됩니다.
- 결과 $\hat{y}$도 8개의 예측값을 가집니다.

### Step 2: Calculate Cost (MSE)

- 데이터가 여러 개이므로 **평균 제곱 오차(Mean Squared Error, MSE)**를 사용합니다.
  $$ C = \frac{1}{n} \sum (\hat{y}\_i - y_i)^2 $$
- $n$: 데이터 개수 (여기서는 8개)

### Step 3: PyTorch로 Gradient 확인

손으로 유도하기 전에, 우리가 목표로 하는 정답(기울기)을 미리 확인해 둡니다.

```python
import torch

# 데이터 (8개)
x = torch.tensor([0., 1., 2., 3., 4., 5., 6., 7.])
y = torch.tensor([1.86, 1.31, .62, .33, .09, -.67, -1.23, -1.37])

# 파라미터 초기화
m = torch.tensor(0.9, requires_grad=True)
b = torch.tensor(0.1, requires_grad=True)

# Forward Pass
y_hat = m*x + b

# Calculate Cost (MSE)
n = len(x)
mse = torch.sum((y_hat - y)**2) / n
print(f"MSE Cost: {mse.item():.2f}") # 약 19.6x

# Backward Pass
mse.backward()

print(f"dC/dm: {m.grad.item():.2f}") # 약 36.3
print(f"dC/db: {b.grad.item():.2f}") # 약 6.3
```

우리의 목표는 이제 다음 장에서 수식을 유도하여 **36.3**과 **6.3**이라는 숫자를 **직접 만들어내는 것**입니다.

---

## 🎯 핵심 개념

- **단일 포인트**: 계산은 쉽지만 불안정하고 비효율적이다. (Stochastic)
- **배치(Batch)**: 전체 데이터를 사용해 안정적인 기울기를 얻는다. (Batch Gradient Descent)
- **MSE**: 여러 데이터의 오차를 평균 내어 비용 함수로 사용한다.

다음 장에서는 MSE 비용 함수에 대한 편미분을 직접 수행해 보겠습니다. (약간의 $\sum$ 기호가 등장합니다!)
