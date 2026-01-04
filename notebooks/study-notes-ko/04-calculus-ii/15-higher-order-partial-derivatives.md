# 15. 고계 편미분 (Higher Order Partial Derivatives)

> **"두 번 미분하면 가속도가 보인다"**

미적분 I에서 배운 것처럼, 거리를 미분하면 속도(일계도함수), 속도를 미분하면 가속도(이계도함수)가 됩니다.
편미분에서도 마찬가지입니다. **이계 편미분(Second Partial Derivatives)**은 머신러닝의 **최적화(Optimization)** 과정에서 곡면의 '곡률(Curvature)'을 파악하여 학습 속도를 높이는 데 사용됩니다(예: Newton's Method).

---

## 1️⃣ 예제 함수

$$ z = f(x, y) = x^2 + 5xy + y^2 $$

### 일계 편미분 (First Partial Derivatives)

1. **$\frac{\partial z}{\partial x}$**: ($y$는 상수)
   $$ 2x + 5y $$
2. **$\frac{\partial z}{\partial y}$**: ($x$는 상수)
   $$ 5x + 2y $$

---

## 2️⃣ 이계 편미분 (Second Partial Derivatives)

이미 한 번 미분한 것을 **또 미분**합니다. 두 가지 종류가 있습니다.

### A. 비혼합 (Unmixed)

같은 변수로 두 번 미분합니다.

1. **$\frac{\partial^2 z}{\partial x^2}$** ($= f_{xx}$):

   - $\frac{\partial z}{\partial x} = 2x + 5y$ 를 다시 $x$로 미분.
   - **결과: 2**

2. **$\frac{\partial^2 z}{\partial y^2}$** ($= f_{yy}$):
   - $\frac{\partial z}{\partial y} = 5x + 2y$ 를 다시 $y$로 미분.
   - **결과: 2**

### B. 혼합 (Mixed) ✨ 중요!

다른 변수로 번갈아 미분합니다.

1. **$\frac{\partial^2 z}{\partial y \partial x}$** ($= f_{xy}$):

   - $\frac{\partial z}{\partial x} = 2x + 5y$ 를 $y$로 미분. (순서: $x \to y$)
   - $2x$는 0, $5y$는 5가 됨.
   - **결과: 5**

2. **$\frac{\partial^2 z}{\partial x \partial y}$** ($= f_{yx}$):
   - $\frac{\partial z}{\partial y} = 5x + 2y$ 를 $x$로 미분. (순서: $y \to x$)
   - $5x$는 5, $2y$는 0이 됨.
   - **결과: 5**

> **클레로의 정리 (Clairaut's Theorem)**: 연속 함수라면 미분 순서는 상관없습니다!
> $$ \frac{\partial^2 z}{\partial y \partial x} = \frac{\partial^2 z}{\partial x \partial y} $$

---

## 3️⃣ 표기법 정리

| 의미                   | Leibniz                                      | Function                                     | Subscript | Operator   |
| :--------------------- | :------------------------------------------- | :------------------------------------------- | :-------- | :--------- |
| **비혼합 ($x \to x$)** | $\frac{\partial^2 z}{\partial x^2}$          | $\frac{\partial^2 f}{\partial x^2}$          | $f_{xx}$  | $D_{xx} f$ |
| **혼합 ($x \to y$)**   | $\frac{\partial^2 z}{\partial y \partial x}$ | $\frac{\partial^2 f}{\partial y \partial x}$ | $f_{xy}$  | $D_{yx} f$ |

> **주의**: Leibniz 표기법($\frac{\partial^2 z}{\partial y \partial x}$)은 오른쪽($x$)부터 읽지만, Subscript($f_{xy}$)는 왼쪽($x$)부터 읽습니다. (순서 주의!)

---

## 🎯 Wrap-up: 편미분(Partial Derivatives) 파트 완료!

이로써 **"Calculus II: 2. Partial Derivatives"** 섹션을 모두 마쳤습니다.
우리는 다음을 배웠습니다:

- 편미분의 개념과 계산 ($\partial$)
- 기울기(Gradient)와 경사 하강법(Gradient Descent)
- 연쇄 법칙(Chain Rule)과 역전파(Backpropagation)
- 고계 편미분과 클레로의 정리

이제 마지막 파트, **"3. 적분 (Integrals)"**으로 나아갑니다! 🏔️
