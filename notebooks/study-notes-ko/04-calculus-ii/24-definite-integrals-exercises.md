# 24. 정적분 연습 문제 (Definite Integral Exercises)

> **"손으로 풀고, 파이썬으로 검증하기"**

이번에는 정적분 문제를 손으로 직접 풀어보고(Symbolic), 파이썬 코드로 검증(Numerical)하는 과정을 연습해 봅니다.

---

## 📝 문제

$$ \int\_{3}^{4} 2x dx $$

---

## ✍️ 방법 1: 손으로 풀기 (Symbolic Integration)

### Step 1: 부정적분 구하기

- 함수 $2x$ 적분:
  $$ \int 2x dx = 2 \int x dx = 2 \left( \frac{x^2}{2} \right) + C = x^2 + C $$

### Step 2: 위끝 ($x=4$) 대입

- $F(4) = 4^2 + C = 16 + C$

### Step 3: 아래끝 ($x=3$) 대입

- $F(3) = 3^2 + C = 9 + C$

### Step 4: 뺄셈

$$ F(4) - F(3) = (16 + C) - (9 + C) = 16 - 9 = \mathbf{7} $$

---

## 🐍 방법 2: 파이썬으로 풀기 (Numerical Integration)

`scipy.integrate.quad`를 사용해 컴퓨터가 계산한 결과와 비교해 봅니다.

```python
from scipy.integrate import quad

# 1. 함수 정의 (y = 2x)
def h(x):
    return 2 * x

# 2. 정적분 수행 (3부터 4까지)
result = quad(h, 3, 4)

print(result)
```

### 실행 결과

```
(7.0, 7.771561172376096e-14)
```

- **결과**: `7.0` (손으로 푼 결과와 정확히 일치)
- **오차**: 거의 0에 수렴

---

## 🎯 결론

수학 공식을 몰라도 파이썬을 이용하면 복잡한 적분 값(면적)을 아주 쉽고 빠르게 구할 수 있습니다.
다음 시간에는 **"ROC 곡선 아래 면적(AUC)"**을 이 적분을 이용해 구하는 방법을 알아보겠습니다.
