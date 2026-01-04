# 2. 편미분 (Partial Derivatives)

> **"변수가 여러 개일 때, 하나씩 쪼개서 미분하기"**

머신러닝 모델은 수많은 변수(파라미터)를 가집니다.
$y = mx + b$에서도 변수는 $m$과 $b$ 두 개였죠. 변수가 여러 개인 **다변수 함수(Multivariate Function)**에서, 특정 변수에 대한 기울기를 구하는 방법이 바로 **편미분(Partial Derivative)**입니다.

---

## 1️⃣ 다변수 함수 예시: $z = x^2 - y^2$

이 함수는 3차원 공간에서 **말안장(Saddle Point)** 모양의 그래프를 그립니다.
($x$축으로는 위로 열린 포물선, $y$축으로는 아래로 열린 포물선)

### 시각화 (GeoGebra/Python)

- **GeoGebra**: 3D 계산기에서 `z = x^2 - y^2`를 입력하면 말안장 모양을 돌려가며 볼 수 있습니다.
- **Python**: `matplotlib`을 이용해 단면을 잘라서 2D 그래프로 분석할 수 있습니다.

---

## 2️⃣ 편미분 기호 ($\partial$)

- 일반 미분($d$)과 구분하기 위해 **$\partial$** (del, partial, rounded d) 기호를 사용합니다. (니콜라 드 콩도르세가 제안)
- $\frac{\partial z}{\partial x}$: $x$에 대한 $z$의 편미분 (y는 상수로 취급)
- $\frac{\partial z}{\partial y}$: $y$에 대한 $z$의 편미분 (x는 상수로 취급)

---

## 3️⃣ 계산 방법: "나머지는 모두 상수 취급"

### Case 1: $x$에 대한 편미분 ($\frac{\partial z}{\partial x}$)

- **전략**: $y$를 상수(숫자)로 생각하고 미분합니다.
- 식: $z = x^2 - y^2$
- 미분:
  - $x^2 \to 2x$ (거듭제곱 법칙)
  - $y^2 \to 0$ (상수니까 미분하면 0)
- 결과: **$\frac{\partial z}{\partial x} = 2x$**
- **의미**: $x$ 방향으로의 기울기는 $x$값의 2배입니다. ($y$값과는 무관!)

### Case 2: $y$에 대한 편미분 ($\frac{\partial z}{\partial y}$)

- **전략**: $x$를 상수로 생각하고 미분합니다.
- 식: $z = x^2 - y^2$
- 미분:
  - $x^2 \to 0$ (상수)
  - $-y^2 \to -2y$
- 결과: **$\frac{\partial z}{\partial y} = -2y$**
- **의미**: $y$ 방향으로의 기울기는 $y$값의 -2배입니다.

---

## 4️⃣ Python 코드로 확인

```python
import numpy as np
import matplotlib.pyplot as plt

def f(x, y):
    return x**2 - y**2

# Case 1: y를 0으로 고정하고 x에 따른 변화 관찰
x = np.linspace(-3, 3, 1000)
y_fixed = 0
z_x = f(x, y_fixed)

plt.plot(x, z_x)
plt.title("z with respect to x (y=0)")
# 결과: z = x^2 (아래로 볼록한 포물선), 기울기는 2x

# Case 2: x를 0으로 고정하고 y에 따른 변화 관찰
y = np.linspace(-3, 3, 1000)
x_fixed = 0
z_y = f(x_fixed, y)

plt.plot(y, z_y)
plt.title("z with respect to y (x=0)")
# 결과: z = -y^2 (위로 볼록한 포물선), 기울기는 -2y
```

---

## 🎯 요약

- **편미분**은 다변수 함수에서 **한 변수만 남기고 나머지는 상수 취급**하여 미분하는 것입니다.
- $\frac{\partial z}{\partial x} = 2x$: $x$가 변할 때 $z$의 변화율.
- $\frac{\partial z}{\partial y} = -2y$: $y$가 변할 때 $z$의 변화율.

다음 장에서는 편미분을 실제 문제에 적용해보는 연습 문제를 풀어보겠습니다.
