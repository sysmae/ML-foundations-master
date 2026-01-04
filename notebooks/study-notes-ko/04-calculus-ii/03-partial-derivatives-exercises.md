# 3. 편미분 연습 문제 (Partial Derivatives Exercises)

> **"직접 계산해보며 편미분 감각 익히기"**

이번 장에서는 함수 $z = x^2 - y^2$를 이용하여, 특정 지점($x, y$)에서의 함수값($z$)과 기울기($\frac{\partial z}{\partial x}, \frac{\partial z}{\partial y}$)를 직접 계산해 봅니다.

**기본 공식**:

- $z = x^2 - y^2$
- $\frac{\partial z}{\partial x} = 2x$
- $\frac{\partial z}{\partial y} = -2y$

---

## 📝 문제 1: $x=3, y=0$

1. **함수값 $z$ 계산**:
   $$ z = 3^2 - 0^2 = 9 - 0 = \mathbf{9} $$
2. **$x$에 대한 기울기 ($\frac{\partial z}{\partial x}$)**:
   $$ 2(3) = \mathbf{6} $$
3. **$y$에 대한 기울기 ($\frac{\partial z}{\partial y}$)**:
   $$ -2(0) = \mathbf{0} $$

**해석**: 이 지점(3, 0)에서 $x$축 방향으로는 가파른 오르막(기울기 6)이지만, $y$축 방향으로는 평지(기울기 0)입니다.

---

## 📝 문제 2: $x=2, y=3$

1. **함수값 $z$ 계산**:
   $$ z = 2^2 - 3^2 = 4 - 9 = \mathbf{-5} $$
2. **$x$에 대한 기울기**:
   $$ 2(2) = \mathbf{4} $$
3. **$y$에 대한 기울기**:
   $$ -2(3) = \mathbf{-6} $$

**해석**: $x$축으로는 오르막(4), $y$축으로는 가파른 내리막(-6)입니다. 말안장의 특성이 잘 드러납니다.

---

## 📝 문제 3: $x=-2, y=-3$

1. **함수값 $z$ 계산**:
   $$ z = (-2)^2 - (-3)^2 = 4 - 9 = \mathbf{-5} $$
2. **$x$에 대한 기울기**:
   $$ 2(-2) = \mathbf{-4} $$
3. **$y$에 대한 기울기**:
   $$ -2(-3) = \mathbf{6} $$

**해석**: 원점을 기준으로 문제 2와 대칭적인 위치이며, 기울기의 부호가 반대입니다($x$는 내리막, $y$는 오르막).

---

## 💻 Python 검증 코드

아래 코드로 위 계산 결과를 시각적으로 확인할 수 있습니다.

```python
import numpy as np

def calculate_gradients(x_val, y_val):
    z = x_val**2 - y_val**2
    dz_dx = 2 * x_val
    dz_dy = -2 * y_val

    print(f"Point ({x_val}, {y_val}):")
    print(f"  z = {z}")
    print(f"  dz/dx (Slope x) = {dz_dx}")
    print(f"  dz/dy (Slope y) = {dz_dy}")
    print("-" * 30)

# 연습 문제 검증
calculate_gradients(3, 0)
calculate_gradients(2, 3)
calculate_gradients(-2, -3)
```

---

## 🎯 결론

- **수기 계산**은 편미분의 기하학적 의미를 이해하는 데 매우 중요합니다.
- 하지만 변수가 수백만 개인 딥러닝에서는 손으로 풀 수 없겠죠?
- 다음 장에서는 **연쇄 법칙(Chain Rule)**이 편미분에서 어떻게 적용되는지 알아보겠습니다. (이것이 머신러닝의 핵심!)
