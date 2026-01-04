# 4. PyTorch로 편미분 구하기 (Partial Derivatives with PyTorch)

> **"복잡한 편미분도 코드 몇 줄로 해결"**

이전 장에서 손으로 계산했던 편미분을 **PyTorch 자동 미분(Autodiff)**을 이용해 계산해 봅시다.
변수가 $x, y$ 두 개로 늘어났지만, 원리는 변수가 하나였을 때와 똑같습니다.

---

## 1️⃣ 기본 원리

1. **변수 생성**: $x, y$ 텐서를 만들고 `requires_grad=True`로 설정합니다.
2. **함수 정의**: $z = x^2 - y^2$ (Forward Pass).
3. **미분 실행**: `z.backward()` (Backward Pass).
4. **기울기 확인**: `x.grad`, `y.grad`로 각각의 편미분 값을 확인합니다.

---

## 2️⃣ 코드 예시: Point (0, 0)

원점(0, 0)에서의 기울기를 구해봅시다.
($\frac{\partial z}{\partial x} = 2(0) = 0$, $\frac{\partial z}{\partial y} = -2(0) = 0$이 나와야 합니다.)

```python
import torch

# 1. 변수 생성 (requires_grad=True 필수!)
x = torch.tensor(0.0, requires_grad=True)
y = torch.tensor(0.0, requires_grad=True)

# 2. Forward Pass (함수 계산)
z = x**2 - y**2
print(f"z value: {z}")
# tensor(0., grad_fn=<SubBackward0>)

# 3. Backward Pass (역전파)
z.backward()

# 4. 기울기 확인
print(f"dz/dx: {x.grad}") # tensor(0.)
print(f"dz/dy: {y.grad}") # tensor(0.)
```

예상대로 모두 0이 나옵니다.

---

## 📝 연습 문제: 이전 문제 다시 풀기

이전 장에서 손으로 풀었던 문제들을 PyTorch로 검증해 보세요.

### 문제 1: Point (3, 0)

- 손 계산: $\frac{\partial z}{\partial x}=6, \frac{\partial z}{\partial y}=0$

```python
x = torch.tensor(3.0, requires_grad=True)
y = torch.tensor(0.0, requires_grad=True)
z = x**2 - y**2
z.backward()
print(x.grad, y.grad) # tensor(6.), tensor(0.)
```

### 문제 2: Point (2, 3)

- 손 계산: $\frac{\partial z}{\partial x}=4, \frac{\partial z}{\partial y}=-6$

```python
x = torch.tensor(2.0, requires_grad=True)
y = torch.tensor(3.0, requires_grad=True)
z = x**2 - y**2
z.backward()
print(x.grad, y.grad) # tensor(4.), tensor(-6.)
```

---

## 🎯 핵심 포인트

- 변수가 2개든, 100만 개든 `backward()`를 호출하면 **모든 `requires_grad=True` 변수들에 대한 편미분 값**이 한 번에 계산됩니다.
- 이것이 머신러닝에서 파라미터가 수억 개라도 학습이 가능한 이유입니다.

다음 장에서는 $z = x^2 - y^2$보다 조금 더 복잡한 함수들을 다루며 **편미분의 연쇄 법칙** 등을 익혀보겠습니다.
