# 17. 종합 연습 문제 (Comprehensive Exercises)

> **"곱, 몫, 연쇄 법칙을 모두 사용하는 고난이도 문제들"**

이번 장에서는 지금까지 배운 모든 도함수 규칙(Product, Quotient, Chain Rule)을 활용하여 5개의 심화 문제를 풀어봅니다.

---

## 📝 문제 1: 곱의 법칙 (Product Rule)

$$ y = (2x^2 + 6x)(2x^3 + 5x^2) $$

- **분해**:
  - $w = 2x^2 + 6x \quad \Rightarrow \quad w' = 4x + 6$
  - $z = 2x^3 + 5x^2 \quad \Rightarrow \quad z' = 6x^2 + 10x$
- **곱의 법칙**: $y' = w z' + z w'$
- **대입**:
  $$ (2x^2 + 6x)(6x^2 + 10x) + (2x^3 + 5x^2)(4x + 6) $$
  (전개하여 정리하면 최종 답이 나오지만, 보통 이 형태로 두어도 무방합니다.)

---

## 📝 문제 2: 몫의 법칙 (Quotient Rule)

$$ y = \frac{6x^2}{2-x} $$

- **분해**:
  - $w = 6x^2 \quad \Rightarrow \quad w' = 12x$ (분자)
  - $z = 2-x \quad \Rightarrow \quad z' = -1$ (분모)
- **몫의 법칙**: $y' = \frac{z w' - w z'}{z^2}$
- **대입**:
  $$ \frac{(2-x)(12x) - (6x^2)(-1)}{(2-x)^2} $$
- **정리**:
  분자 = $(24x - 12x^2) + 6x^2 = 24x - 6x^2$
  $$ y' = \frac{-6x^2 + 24x}{(2-x)^2} $$

---

## 📝 문제 3: 연쇄 법칙 (Chain Rule) - 기본

$$ y = (3x + 1)^2 $$

- **분해**:
  - 안쪽($u$) = $3x + 1 \quad \Rightarrow \quad u' = 3$
  - 바깥($y$) = $u^2 \quad \Rightarrow \quad \frac{dy}{du} = 2u = 2(3x+1) = 6x+2$
- **연쇄 법칙**: $y' = \frac{dy}{du} \cdot \frac{du}{dx}$
- **계산**:
  $$ (6x + 2) \cdot 3 = 18x + 6 $$

---

## 📝 문제 4: 연쇄 법칙 (Chain Rule) - 심화

$$ y = (x^2 + 5x)^6 $$

- **분해**:
  - $u = x^2 + 5x \quad \Rightarrow \quad u' = 2x + 5$
  - $y = u^6 \quad \Rightarrow \quad \frac{dy}{du} = 6u^5 = 6(x^2+5x)^5$
- **연쇄 법칙**:
  $$ y' = 6(x^2 + 5x)^5 \cdot (2x + 5) $$

---

## 📝 문제 5: 다중 연쇄 법칙 (Nested Chain Rule)

$$ y = \frac{1}{(x^4+1)^5 + 7} $$

이런 복잡한 식은 **3단 분리**가 필요합니다.

1. **가장 안쪽 ($t$)**: $t = x^4 + 1$
   - $t' = 4x^3$
2. **중간 ($u$)**: $u = t^5 + 7$
   - $\frac{du}{dt} = 5t^4$
3. **가장 바깥 ($y$)**: $y = \frac{1}{u} = u^{-1}$
   - $\frac{dy}{du} = -u^{-2} = -\frac{1}{u^2}$

- **연쇄 법칙 (Chain of Chain)**:
  $$ \frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dt} \cdot \frac{dt}{dx} $$

- **대입**:
  $$ \left( -\frac{1}{((x^4+1)^5+7)^2} \right) \cdot (5(x^4+1)^4) \cdot (4x^3) $$

---

## 🎯 결론

복잡해 보이는 함수도 **블록 조립하듯** 분해하면 미분할 수 있습니다.
이제 **자동 미분(Automatic Differentiation)**으로 넘어가기 전, 마지막 법칙을 하나만 더 배우겠습니다.
