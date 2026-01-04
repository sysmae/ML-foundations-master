# 5. 실린더 부피로 배우는 편미분 (Partial Derivatives with Cylinder Volume)

> **"기하학적 예시로 이해하는 편미분의 의미"**

이번 장에서는 **실린더(원기둥)의 부피(Volume)** 공식을 이용하여 편미분이 실제 기하학적 세계에서 어떤 의미를 갖는지 알아봅니다.

---

## 1️⃣ 실린더 부피 공식

$$ V = \pi r^2 l $$

- $r$: 반지름 (radius)
- $l$: 길이/높이 (length)
- $V$: 부피 (volume)

이 함수는 $r$과 $l$이라는 **두 개의 변수**에 의해 결정되는 다변수 함수입니다.

---

## 2️⃣ 길이에 대한 편미분 ($\frac{\partial V}{\partial l}$)

- **가정**: 파이프의 굵기(반지름 $r$)는 고정하고, 길이($l$)만 늘린다면 부피는 어떻게 변할까?
- **미분**: $l$을 제외한 나머지는 상수 취급합니다.
  $$ \frac{\partial V}{\partial l} = \pi r^2 \cdot \frac{\partial}{\partial l}(l) = \pi r^2 \cdot 1 = \pi r^2 $$
- **의미**: 길이가 1만큼 늘어날 때, 부피는 **단면적($\pi r^2$)만큼** 늘어납니다. ($l$값 자체와는 무관하게 일정함)

### PyTorch 검증 ($r=3, l=5$)

- 이론값: $\pi (3^2) = 9\pi \approx 28.3$

```python
import torch
import math

r = torch.tensor(3.0, requires_grad=True)
l = torch.tensor(5.0, requires_grad=True)

v = math.pi * r**2 * l
v.backward()

print(f"Volume: {v.item():.1f}")        # 141.4
print(f"dV/dl: {l.grad.item():.1f}")    # 28.3 (약 9pi)
```

---

## 3️⃣ 반지름에 대한 편미분 ($\frac{\partial V}{\partial r}$)

- **가정**: 파이프의 길이($l$)는 고정하고, 굵기($r$)만 키운다면 부피는 어떻게 변할까?
- **미분**: $r$을 제외한 나머지는 상수 취급합니다.
  $$ \frac{\partial V}{\partial r} = \pi l \cdot \frac{\partial}{\partial r}(r^2) = \pi l \cdot 2r = 2\pi r l $$
- **의미**: 반지름이 늘어날 때 부피의 변화율은 **현재의 반지름($r$)과 길이($l$)에 비례**하여 커집니다. (선형적이지 않음!)

### PyTorch 검증 ($r=3, l=5$)

- 이론값: $2\pi (3)(5) = 30\pi \approx 94.2$

```python
# 위 코드에 이어서
print(f"dV/dr: {r.grad.item():.1f}")    # 94.2 (약 30pi)
```

---

## 4️⃣ 미소 변화량 ($\Delta$) 실험

편미분은 **극한(Limit)**의 개념이므로, 변화량($\Delta r$)을 아주 작게 줄였을 때만 정확히 맞아떨어집니다.

- $\Delta r$이 작을 때: $\frac{V(r+\Delta r) - V(r)}{\Delta r} \approx \frac{\partial V}{\partial r}$ (성립)
- $\Delta r$이 클 때: $V$는 $r^2$에 비례하므로 오차가 커집니다.

```python
# 델타 방법을 이용한 근사 (Numerical Differentiation)
def cylinder_vol(r, l):
    return math.pi * r**2 * l

r_val = 3.0
l_val = 5.0
delta = 0.000001 # 매우 작은 변화

v_original = cylinder_vol(r_val, l_val)
v_delta = cylinder_vol(r_val + delta, l_val)

slope = (v_delta - v_original) / delta
print(f"Numerical dV/dr: {slope:.1f}") # 94.2 (Autodiff 결과와 동일)
```

---

## 🎯 요약

- **$\frac{\partial V}{\partial l} = \pi r^2$**: 길이에 따른 부피 변화율은 단면적과 같습니다. (상수적 변화)
- **$\frac{\partial V}{\partial r} = 2\pi r l$**: 반지름에 따른 부피 변화율은 둘레($2\pi r$) $\times$ 길이($l$)와 같습니다. (가변적 변화)
- 편미분은 기하학적으로 **"어떤 변수를 건드렸을 때 결과가 얼마나 민감하게 반응하는가?"**를 알려줍니다.

다음 장에서는 더 다양한 기하학 도형을 이용해 편미분을 연습해 보겠습니다.
