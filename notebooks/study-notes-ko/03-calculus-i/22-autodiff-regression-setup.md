# 22. 자동 미분을 이용한 회귀 분석 준비 (Regression Setup with Autodiff)

> **"간단한 직선 맞추기로 시작하는 머신러닝"**

이번 장에서는 자동 미분(Autodiff)을 활용하여 간단한 **선형 회귀(Linear Regression)** 모델을 만드는 준비 과정을 다룹니다.
우리의 목표는 주어진 데이터에 가장 잘 맞는 직선 $y = mx + b$를 찾는 것입니다.

---

## 1️⃣ 계산 그래프 (Computational Graph)

우리가 만들 모델은 $y = mx + b$ 형태의 직선입니다. 이를 유향 비순환 그래프(DAG)로 표현하면 다음과 같습니다.

- **노드(Node)**: 변수($x, y, m, b$)와 연산(곱셈 $\times$, 덧셈 $+$)
- **엣지(Edge)**: 데이터의 흐름 (Tensor)

1. 입력($x$)과 기울기($m$)를 곱함 $\to mx$
2. 결과($mx$)에 절편($b$)을 더함 $\to mx + b$
3. 최종 출력($y$) 생성

---

## 2️⃣ 데이터 생성 (Data Generation)

가상의 데이터를 만들어 봅시다. (예: 알츠하이머 약물 투여량 vs. 건망증 빈도)
현실성을 위해 **노이즈(Noise)**를 추가합니다.

```python
import torch
import matplotlib.pyplot as plt

# 데이터 생성 함수
def regression_data(m=-0.5, b=2.0, noise_std=0.2):
    x = torch.rand(8) * 10  # 0~10 사이의 랜덤 x값 8개
    noise = torch.randn(8) * noise_std
    y = m * x + b + noise
    return x, y

# 고정된 데이터를 사용 (재현성을 위해)
x = torch.tensor([0., 1., 2., 3., 4., 5., 6., 7.])
# y값은 위 함수로 생성했다고 가정 (약의 양이 늘면 건망증이 줄어드는 음의 상관관계)
y = torch.tensor([1.86, 1.31, .62, .33, .09, -.67, -1.23, -1.37])

plt.scatter(x, y)
plt.xlabel("Drug dosage")
plt.ylabel("Forgetfulness")
plt.show()
```

데이터는 우하향하는(음의 기울기) 경향을 보입니다.

---

## 3️⃣ 파라미터 초기화 (Initialization)

우리가 찾아야 할 파라미터는 기울기($m$)와 절편($b$)입니다.
머신러닝에서는 보통 이 값들을 **랜덤한 작은 수**로 초기화한 뒤 학습시킵니다.

```python
# 랜덤 초기화 (0에 가까운 임의의 값)
# requires_grad=True 필수! (미분을 통해 업데이트할 것이므로)
m = torch.tensor([0.9], requires_grad=True)
b = torch.tensor([0.1], requires_grad=True)

# 회귀 모델 정의
def regression(x, m, b):
    return m * x + b

# 현재 모델의 예측값 확인 (학습 전)
y_pred = regression(x, m, b)

# 시각화하면 데이터와 전혀 맞지 않는 직선이 그려집니다.
```

### 왜 랜덤으로 시작하나요?

- 통계적 방법(LSE 등)이나 선형대수(유사역행렬)로 한 번에 정답을 구할 수도 있지만, 이 방법은 데이터가 수억 개일 때는 불가능합니다.
- **경사 하강법(Gradient Descent)**은 무작위 상태에서 시작해 조금씩 정답을 찾아가는 방식이므로, 어떤 규모의 모델에도 적용 가능합니다.

---

## 🎯 요약

1. **목표**: 데이터($x, y$)에 맞는 $m$과 $b$ 찾기.
2. **준비**: $m, b$를 `requires_grad=True`인 텐서로 만들고 랜덤 초기화.
3. **현황**: 초기화된 모델은 데이터와 전혀 맞지 않음 (엉터리 예측).

다음 장에서는 **손실 함수(Cost Function)**를 정의하고, 자동 미분을 이용해 모델을 학습시키는 과정을 배워보겠습니다.
