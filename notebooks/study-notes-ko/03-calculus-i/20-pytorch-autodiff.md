# 20. PyTorch 자동 미분 (Autodiff with PyTorch)

> **"requires_grad와 backward()로 미분 끝!"**

이번 장에서는 가장 인기 있는 딥러닝 라이브러리 중 하나인 **O'Reilly PyTorch**를 사용하여 미분을 계산해 봅니다.
손으로 풀었던 $y=x^2$ 문제를 코드로 풀어보며 기본 사용법을 익혀봅시다.

---

## 1️⃣ 문제 정의

함수 $y = x^2$에서 $x=5$일 때의 기울기($\frac{dy}{dx}$)를 구하시오.

- **수기 계산**:
  - $y' = 2x$
  - $x=5$ 대입 $\to 2(5) = 10$
- **정답**: **10**

---

## 2️⃣ PyTorch 구현

PyTorch의 `autograd` 기능을 사용하면 이 과정을 자동으로 처리할 수 있습니다.

### 단계 1: 텐서 생성 및 Gradient 추적 설정

```python
import torch

# x = 5.0 생성 (반드시 실수형 float여야 함)
# requires_grad=True: 이 텐서에 대한 연산을 추적하여 나중에 미분을 계산하겠다는 뜻
x = torch.tensor(5.0, requires_grad=True)

print(x)
# tensor(5., requires_grad=True)
```

### 단계 2: Forward Pass (함수 계산)

```python
y = x**2

print(y)
# tensor(25., grad_fn=<PowBackward0>)
# grad_fn은 이 텐서가 어떤 연산(여기서는 Power)을 통해 만들어졌는지 기억합니다.
```

### 단계 3: Backward Pass (역전파)

이제 $y$를 기준으로 $x$에 대한 미분값을 계산합니다.

```python
y.backward()
```

이 한 줄의 코드가 연쇄 법칙을 적용하여 모든 기울기를 계산합니다.

### 단계 4: 기울기 확인

계산된 미분값은 `x.grad` 속성에 저장됩니다.

```python
print(x.grad)
# tensor(10.)
```

우리가 예상한 값 **10**과 정확히 일치합니다!

---

## 🎯 핵심 개념

1. **`requires_grad=True`**:
   - 텐서를 생성할 때 이 옵션을 켜야 미분(Gradient)을 추적합니다.
   - 메모리를 더 사용하므로, 학습이 필요한 파라미터(Weight, Bias)에만 사용합니다.
2. **`backward()`**:
   - 최종 결과값(주로 Loss)에서 호출하며, 역전파를 수행하여 연결된 모든 `requires_grad=True` 텐서의 기울기를 계산합니다.
3. **`.grad`**:
   - 계산된 기울기 값이 저장되는 속성입니다.

다음 영상에서는 또 다른 주요 라이브러리인 **TensorFlow**를 사용하여 동일한 작업을 수행해 보겠습니다.
