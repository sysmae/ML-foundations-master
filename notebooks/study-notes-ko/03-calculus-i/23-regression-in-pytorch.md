# 23. PyTorch로 회귀 모델 학습하기 (Training Regression Model)

> **"4단계 머신러닝 루프를 통한 자동 학습의 원리"**

이번 장은 미적분학 I 과목의 하이라이트입니다.
앞서 준비한 **선형 회귀(Linear Regression)** 모델($y = mx + b$)을, 자동 미분을 활용한 **4단계 과정**을 통해 실제로 학습시켜 봅니다.
이 과정은 단순한 회귀뿐만 아니라, 가장 복잡한 딥러닝 모델까지 모두 동일하게 적용되는 머신러닝의 핵심 원리입니다.

---

## 1️⃣ 머신러닝의 4단계 (The 4-Step Process)

머신러닝 모델이 학습하는 과정은 다음 4단계의 반복(Loop)으로 정의됩니다.

### Step 1: Forward Pass (순전파)

- **개념**: 입력 데이터($x$)를 모델에 통과시켜 예측값($\hat{y}$)을 구하는 과정입니다.
- **수식**: $\hat{y} = mx + b$
- 초기에는 파라미터($m, b$)가 랜덤하기 때문에 예측값은 엉터리일 것입니다.

### Step 2: Calculate Cost (비용 계산)

- **개념**: 예측값($\hat{y}$)이 실제 정답($y$)과 얼마나 다른지 **오차(Error)**를 수치화합니다.
- **함수**: **평균 제곱 오차(MSE, Mean Squared Error)**를 사용합니다.
  - 단순히 차이($\hat{y} - y$)만 구하면 양수/음수가 상쇄될 수 있으므로 **제곱**하여 더합니다.
  - 제곱을 하면 오차가 클수록 비용이 기하급수적으로 커져 모델에게 강력한 패널티를 줍니다.
- **수식**: $C = \frac{1}{n} \sum (\hat{y}_i - y_i)^2$

### Step 3: Backward Pass (역전파)

- **개념**: "비용을 줄이려면 파라미터($m, b$)를 어느 방향으로, 얼마나 수정해야 하는가?"를 알아내는 과정입니다.
- **방법**: 비용 함수($C$)에 대한 각 파라미터의 **기울기(Gradient, $\frac{\partial C}{\partial m}, \frac{\partial C}{\partial b}$)**를 계산합니다.
- **도구**: 여기서 **자동 미분(Autodiff)**이 사용됩니다. 우리가 직접 미분할 필요 없이 `C.backward()` 한 번이면 끝납니다.

### Step 4: Optimization (최적화)

- **개념**: 계산된 기울기 반대 방향으로 파라미터를 조금씩 이동시켜 비용을 줄입니다.
- **알고리즘**: **경사 하강법(Gradient Descent)**, 그중에서도 **SGD(확률적 경사 하강법)**를 주로 사용합니다.
- **학습률(Learning Rate)**: 한 번에 얼마나 이동할지 보폭을 결정하는 중요한 하이퍼파라미터입니다.

---

## 2️⃣ PyTorch 구현 상세

### 2.1 준비 (Setup)

이전 장에서 만든 데이터와 초기화된 파라미터를 가져옵니다.

```python
import torch

# 데이터 (약물 투여량 vs 건망증)
x = torch.tensor([0., 1., 2., 3., 4., 5., 6., 7.])
y = torch.tensor([1.86, 1.31, .62, .33, .09, -.67, -1.23, -1.37])

# 파라미터 초기화 (랜덤 값, 미분 추적 켜기)
m = torch.tensor([0.9], requires_grad=True)
b = torch.tensor([0.1], requires_grad=True)
```

### 2.2 함수 정의

```python
# 1. 모델 함수 (Forward)
def regression(x, m, b):
    return m * x + b

# 2. 비용 함수 (MSE)
def mse(y_hat, y):
    sigma = torch.sum((y_hat - y)**2)
    return sigma / len(y)

# 3. 최적화 도구 (Optimizer) 설정
# SGD를 사용하며, 학습률(lr)은 0.01로 설정합니다.
# params=[m, b]는 "이 변수들을 수정해서 비용을 줄여라"는 의미입니다.
optimizer = torch.optim.SGD([m, b], lr=0.01)
```

### 2.3 학습 루프 (Training Loop)

1000번 반복(Epoch)하여 점진적으로 정답을 찾아갑니다.

```python
epochs = 1000

for epoch in range(epochs):

    # 0. 기울기 초기화
    # PyTorch는 기울기를 누적(accumulate)하는 특성이 있어, 매 반복마다 0으로 리셋해야 합니다.
    optimizer.zero_grad()

    # Step 1: Forward Pass
    y_hat = regression(x, m, b)

    # Step 2: Calculate Cost
    C = mse(y_hat, y)

    # Step 3: Backward Pass
    # 비용 C에서 시작하여 연결된 m, b까지 역으로 미분을 수행합니다.
    C.backward()

    # Step 4: Optimization
    # 계산된 기울기(grad)를 바탕으로 m, b 값을 업데이트합니다.
    optimizer.step()

    # 로그 출력 (100번마다)
    if (epoch + 1) % 100 == 0:
        print(f"Epoch {epoch+1}: Cost = {C.item():.4f}, m = {m.item():.4f}, b = {b.item():.4f}")
```

### 학습 과정 해설

1. **초기 상태**: Cost가 매우 높음 (예: 19.xx). 기울기(Gradient)는 비용을 줄이는 방향을 가리킵니다.
2. **학습 중반**: Cost가 급격히 떨어짐. $m, b$ 값이 조금씩 변함 (예: $m$은 감소, $b$는 증가).
3. **학습 후반**: Cost가 더 이상 줄어들지 않고 특정 값에 수렴. 이때의 $m, b$가 최적의 모델입니다.

---

## 3️⃣ 결과 분석

학습이 끝난 후 최종 파라미터를 확인해 봅시다.

```python
print(f"최종 결과: m = {m.item():.4f}, b = {b.item():.4f}")
```

- 결과값은 대략 **$m \approx -0.47, b \approx 1.75$** 정도가 나옵니다.
- 우리가 데이터를 생성할 때 썼던 실제 값($m=-0.5, b=2.0$)과 비슷하지만, **노이즈(Noise)** 때문에 완벽히 같지는 않습니다.
- 데이터 개수가 8개뿐이라 오차가 있지만, 데이터가 많아질수록 실제 값에 더 가까워집니다.

---

## 4️⃣ 연습 문제 및 마무리

### 연습 문제 1: $y = x^2 + 2x + 2$ 기울기 구하기

PyTorch를 이용해 이 곡선의 $x=2$에서의 접선 기울기를 구해보세요.

```python
x = torch.tensor(2.0, requires_grad=True)
y = x**2 + 2*x + 2
y.backward()
print(x.grad) # 6.0 출력 예상
```

이전에 **델타 방법**으로 구했던 값(6)과 정확히 일치함을 알 수 있습니다.

### 마무리: 미분 가능한 프로그래밍 (Differentiable Programming)

- 우리는 단순히 수학 식을 미분한 것이 아니라, **파이썬 코드 자체를 미분**했습니다.
- 이는 미래의 프로그래밍 패러다임으로, 코드의 결과(출력)를 원하는 대로 만들기 위해 입력값을 역으로 추적하여수정하는 혁신적인 방식입니다.

이것으로 **Calculus I** 과목을 모두 마칩니다. 다음 과목인 **Calculus II**에서는 변수가 여러 개인 **편미분(Partial Derivatives)**을 배워 실제 딥러닝에 한 걸음 더 다가갑니다.
