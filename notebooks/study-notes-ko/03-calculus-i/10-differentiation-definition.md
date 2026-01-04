# 10. 미분의 정의 (Definition of Differentiation)

> **"델타 방법에서 도함수의 공식으로"**

이번 장에서는 델타 방법을 수학적으로 일반화하여, 우리가 교과서에서 보는 **미분의 정의(First Principles of Differentiation)**를 도출해냅니다.

---

## 1️⃣ 도함수의 공식 유도

이전 장에서 배운 델타 방법의 기울기 공식을 다시 봅시다.

$$
m = \frac{y_2 - y_1}{x_2 - x_1} = \frac{\Delta y}{\Delta x}
$$

여기서:

- $y_1 = f(x)$
- $x_2 = x + \Delta x$
- $y_2 = f(x + \Delta x)$
- $x_2 - x_1 = \Delta x$

이것들을 대입하면:

$$
\frac{\Delta y}{\Delta x} = \frac{f(x + \Delta x) - f(x)}{\Delta x}
$$

이제 $\Delta x \to 0$ 극한을 취하면, 이것이 바로 **도함수(Derivative)**의 정의가 됩니다.

$$
\frac{dy}{dx} = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x}
$$

- **$dy/dx$**: 라이프니츠 표기법으로, "x의 변화에 대한 y의 변화율(미분계수)"을 의미합니다.

---

## 2️⃣ 파이썬 실습: 공식 검증

이 공식을 파이썬 함수로 만들어, $\Delta x$가 줄어들 때 기울기가 어떻게 수렴하는지 확인해 봅시다.

### 실습 함수 정의

```python
def f(x):
    return x**2 + 2*x + 2

def diff_demo(f, x, delta_x):
    return (f(x + delta_x) - f(x)) / delta_x
```

### Case 1: $x = 2$에서의 기울기

$\Delta x$를 점점 줄여가며 계산해 봅니다.

```python
deltas = [1, 0.1, 0.01, 0.001, 0.0001, 0.00001, 0.000001]

print("x = 2일 때의 기울기 변화:")
for delta in deltas:
    m = diff_demo(f, 2, delta)
    print(f"Delta: {delta:.6f}, Slope: {m:.6f}")
```

**예상 결과**:

- Delta가 작아질수록 Slope는 **6**에 가까워집니다.

### Case 2: $x = -1$에서의 기울기

```python
print("\nx = -1일 때의 기울기 변화:")
for delta in deltas:
    m = diff_demo(f, -1, delta)
    print(f"Delta: {delta:.6f}, Slope: {m:.6f}")
```

**예상 결과**:

- Delta가 작아질수록 Slope는 **0**에 가까워집니다.
- 그래프의 최저점(꼭짓점)이므로 기울기가 0인 것이 맞습니다.

---

## 3️⃣ 결론

우리는 두 점 사이의 기울기를 구하는 단순한 아이디어(델타 방법)에서 시작하여, **미분의 가장 일반적인 방정식**을 도출했습니다.

1. **극한 ($\Delta x \to 0$)**
2. **도함수 공식 ($\frac{dy}{dx}$)**

다음 장에서는 **미분의 다양한 표기법**에 대해 간단히 알아보고, 본격적인 미분 규칙(Rule)들을 배울 준비를 합니다.
