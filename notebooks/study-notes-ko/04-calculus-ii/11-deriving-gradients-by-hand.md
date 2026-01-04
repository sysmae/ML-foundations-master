# 11. 기울기 수기 유도 (Deriving Gradients by Hand)

> **"블랙박스를 열다: 자동 미분의 내부 계산 과정"**

이전 장에서 PyTorch가 구해준 기울기 값을 이제 손으로 직접 계산해 봅시다.
(목표: $\frac{\partial C}{\partial m}$과 $\frac{\partial C}{\partial b}$ 구하기)

---

## 1️⃣ 함수 구조 분석 (중첩 함수)

우리의 머신러닝 프로세스는 다음과 같은 **중첩 함수(Nested Functions)** 구조입니다.

1. **외부 함수 ($C \to u$)**:
   $$ C = u^2 $$
   (여기서 $u = \hat{y} - y$라고 둡니다. 즉, **오차**)

2. **내부 함수 ($u \to \hat{y}$)**:
   $$ \hat{y} = mx + b $$
   (여기서 $u = \hat{y} - \text{constant } y$ 이므로, $u$와 $\hat{y}$는 선형 관계)

---

## 2️⃣ Step-by-Step 미분 (Chain Rule 적용)

### Step A. 비용(Cost) 미분하기 ($\frac{\partial C}{\partial \hat{y}}$)

먼저 $\hat{y}$이 변할 때 $C$가 얼마나 변하는지 구합니다.

1. **$\frac{\partial C}{\partial u}$**: $C = u^2$ 미분 $\to 2u$
2. **$\frac{\partial u}{\partial \hat{y}}$**: $u = \hat{y} - y$ 미분 ($y$는 상수) $\to 1$
3. **결합**:
   $$ \frac{\partial C}{\partial \hat{y}} = \frac{\partial C}{\partial u} \cdot \frac{\partial u}{\partial \hat{y}} = 2u \cdot 1 = 2(\hat{y} - y) $$

### Step B. 예측(Prediction) 미분하기 ($\frac{\partial \hat{y}}{\partial m}, \frac{\partial \hat{y}}{\partial b}$)

이제 모델 파라미터가 변할 때 $\hat{y}$이 얼마나 변하는지 구합니다. ($\hat{y} = mx + b$)

1. **$\frac{\partial \hat{y}}{\partial m}$**: $b$와 $x$는 상수 취급.
   $$ mx \to x $$
   $$ b \to 0 $$
   $$ \therefore \frac{\partial \hat{y}}{\partial m} = x $$

2. **$\frac{\partial \hat{y}}{\partial b}$**: $m$과 $x$는 상수 취급.
   $$ mx \to 0 $$
   $$ b \to 1 $$
   $$ \therefore \frac{\partial \hat{y}}{\partial b} = 1 $$

### Step C. 최종 연결 (Chain Rule)

이제 $C$에서 시작해 $m, b$까지의 경로를 연결합니다.

1. **$\frac{\partial C}{\partial m}$**:
   $$ \frac{\partial C}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial m} = 2(\hat{y} - y) \cdot x $$
   **∴ $2x(\hat{y} - y)$\*\*

2. **$\frac{\partial C}{\partial b}$**:
   $$ \frac{\partial C}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial b} = 2(\hat{y} - y) \cdot 1 $$
   **∴ $2(\hat{y} - y)$\*\*

---

## 3️⃣ 수치 대입 및 검증

이제 실제 숫자($x=7, y=-1.37, \hat{y}=6.4$)를 대입해 봅시다.

1. **공통항 ($\hat{y} - y$)**:
   $$ 6.4 - (-1.37) = 7.77 $$
2. **$\frac{\partial C}{\partial m}$ 계산**:
   $$ 2 \cdot x \cdot (7.77) = 2 \cdot 7 \cdot 7.77 = 14 \cdot 7.77 = \mathbf{108.78} $$
   (PyTorch 결과값과 일치!)

3. **$\frac{\partial C}{\partial b}$ 계산**:
   $$ 2 \cdot (7.77) = \mathbf{15.54} $$
   (PyTorch 결과값과 일치!)

---

## 4️⃣ 기울기 벡터 (Gradient Vector, $\nabla C$)

마지막으로 이 두 편미분 값을 하나의 벡터로 묶어서 표현합니다. 이것이 바로 **Gradient**입니다.

$$ \nabla C = \begin{bmatrix} \frac{\partial C}{\partial m} \\ \frac{\partial C}{\partial b} \end{bmatrix} = \begin{bmatrix} 108.78 \\ 15.54 \end{bmatrix} $$

- **$\nabla$ (Nabla)**: 델(Del)이라고도 읽으며, 다변수 함수의 모든 편미분을 벡터로 모아놓은 것을 의미합니다.
- 이 벡터가 가리키는 방향이 **비용(Cost)이 가장 빠르게 증가하는 방향**이며, 우리는 이 **반대 방향**으로 파라미터를 움직여야 합니다(경사 하강법).

다음 영상에서는 이 Gradient를 이용해 실제로 비용을 낮추는 과정을 다룹니다.
