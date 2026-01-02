# 14. 유사역행렬로 선형 회귀 풀기 (Solving Regression with Pseudoinverse)

> **"직선을 데이터에 맞추는 마법 — 머신러닝의 기초"**

이번 장에서는 무어-펜로즈 유사역행렬을 활용하여 **연립방정식의 미지수를 풀고**, **직선을 데이터 좌표에 피팅**하는 방법을 배웁니다. 이것이 바로 **선형 회귀(Linear Regression)**의 핵심 원리입니다!

---

## 1️⃣ 왜 유사역행렬이 머신러닝에서 중요한가?

### 실제 세계의 데이터는 비정방행렬!

집값 예측 모델을 예로 들어봅시다:

```
y = a + b·(침실 수) + c·(학교 거리) + ... + m·(특성 m)
```

| 항목               | 개수         | 설명             |
| :----------------- | :----------- | :--------------- |
| **집 데이터 (행)** | 수천~수만 개 | 훈련 데이터      |
| **특성 (열)**      | 12개 정도    | 침실 수, 거리 등 |
| **미지수**         | 12~13개      | a, b, c, ..., m  |

→ **행 >> 열** = 비정방행렬 = 일반 역행렬 불가!

### 해결책: 유사역행렬

$$
\text{weights } \mathbf{w} = X^+ \mathbf{y}
$$

$X^+$(유사역행렬)를 사용하면 **비정방행렬에서도 미지수를 풀 수 있습니다!**

---

## 2️⃣ 선형 회귀 모델 구조

### 행렬 형태의 회귀 모델

$$
\mathbf{y} = X \mathbf{w}
$$

여기서:

- $\mathbf{y}$: 출력 벡터 (예: 집값) — 크기 $n$
- $X$: 입력 행렬 (예: 특성들) — 크기 $n \times m$
- $\mathbf{w}$: 가중치 벡터 (미지수) — 크기 $m$

### 가중치 구하기

정상적인 역행렬이라면:

$$
\mathbf{w} = X^{-1} \mathbf{y}
$$

하지만 $X$가 비정방행렬이므로:

$$
\mathbf{w} = X^+ \mathbf{y}
$$

---

## 3️⃣ 과결정 vs 불충분결정

### 과결정 시스템 (Overdetermined)

**데이터 수 > 특성 수** (행 > 열)

```
8개 데이터, 2개 특성 → 8×2 행렬
```

- 회귀 모델에서 **가장 흔한 케이스**
- 유사역행렬이 **최소제곱해(Least Squares Solution)** 제공
- $\|X\mathbf{w} - \mathbf{y}\|_2$ 를 최소화하는 $\mathbf{w}$

### 불충분결정 시스템 (Underdetermined)

**데이터 수 < 특성 수** (행 < 열)

```
100개 데이터, 1억 개 특성 → 딥러닝 모델
```

- 딥러닝에서 흔한 케이스
- 유사역행렬이 **최소노름해(Minimum Norm Solution)** 제공
- 가능한 해 중 $\|\mathbf{w}\|_2$ 가 가장 작은 해

---

## 4️⃣ 실습: 약물 복용량과 건망증 회귀 분석

### 문제 설정

치매 치료제의 복용량(x)과 환자의 건망증 정도(y) 관계를 분석합니다.

### 데이터 생성

```python
import numpy as np
import matplotlib.pyplot as plt

# 복용량 (mg): 0~7mg
x = np.array([0, 1, 2, 3, 4, 5, 6, 7])

# 건망증 점수 (낮을수록 좋음)
y = np.array([1.86, 1.31, 0.62, 0.33, 0.09, -0.67, -1.23, -1.37])

print(f"복용량 (x): {x}")
print(f"건망증 (y): {y}")
```

### 데이터 시각화

```python
# 산점도 그리기
plt.figure(figsize=(10, 6))
plt.scatter(x, y, s=100, c='blue', alpha=0.7)
plt.xlabel('복용량 (mg)', fontsize=12)
plt.ylabel('건망증 점수', fontsize=12)
plt.title('치매 치료제 임상 실험', fontsize=14)
plt.grid(True, alpha=0.3)
plt.show()
```

**관찰**: 복용량이 많을수록 건망증 점수가 낮아짐 (= 기억력 개선!)

---

## 5️⃣ 입력 행렬 X 구성하기

### y절편을 위한 열 추가

직선 방정식: $y = b + mx$ (b = y절편, m = 기울기)

행렬 형태로 표현하려면 **상수 열(1로 채워진 열)**이 필요합니다:

$$
\begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{bmatrix}
=
\begin{bmatrix} 1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_n \end{bmatrix}
\begin{bmatrix} b \\ m \end{bmatrix}
$$

### 코드 구현

```python
# x0: y절편을 위한 1로 채워진 열
x0 = np.ones(len(x))
print(f"x0 (절편용): {x0}")
# 출력: [1. 1. 1. 1. 1. 1. 1. 1.]

# x1: 복용량 값
x1 = x
print(f"x1 (복용량): {x1}")
# 출력: [0 1 2 3 4 5 6 7]

# X 행렬 구성: x0과 x1을 열로 결합
X = np.column_stack([x0, x1])
print(f"\n입력 행렬 X (shape: {X.shape}):")
print(X)
# 출력:
# [[1. 0.]
#  [1. 1.]
#  [1. 2.]
#  [1. 3.]
#  [1. 4.]
#  [1. 5.]
#  [1. 6.]
#  [1. 7.]]
```

**해석**:

- 첫 번째 열: 모두 1 → y절편(상수항) 계산용
- 두 번째 열: 복용량 → 기울기 계산용

---

## 6️⃣ 가중치(weights) 계산하기

### 핵심 공식

$$
\mathbf{w} = X^+ \mathbf{y}
$$

### 코드 구현

```python
# 유사역행렬로 가중치 계산
w = np.dot(np.linalg.pinv(X), y)

print(f"가중치 (weights): {w}")
# 출력: [ 1.76  -0.47] (대략)

# 각 가중치의 의미
b = w[0]  # y절편 (intercept)
m = w[1]  # 기울기 (slope)

print(f"\ny절편 (b): {b:.4f}")
print(f"기울기 (m): {m:.4f}")
```

**결과 해석**:

- **y절편 b ≈ 1.76**: 복용량 0일 때 예상 건망증 점수
- **기울기 m ≈ -0.47**: 복용량 1mg 증가 시 건망증 0.47 감소

---

## 7️⃣ 회귀 직선 그리기

### 직선 방정식

$$
y = b + m \cdot x = 1.76 - 0.47x
$$

### 시각화 코드

```python
# x 범위 설정
x_min, x_max = x.min(), x.max()

# 직선의 양 끝점 계산
y_min = m * x_min + b  # 왼쪽 끝점
y_max = m * x_max + b  # 오른쪽 끝점

print(f"직선 시작점: ({x_min}, {y_min:.2f})")
print(f"직선 끝점: ({x_max}, {y_max:.2f})")

# 그래프 그리기
plt.figure(figsize=(10, 6))

# 데이터 산점도
plt.scatter(x, y, s=100, c='blue', alpha=0.7, label='데이터', zorder=5)

# 회귀 직선
plt.plot([x_min, x_max], [y_min, y_max], 'r-', linewidth=2, label='회귀 직선')

plt.xlabel('복용량 (mg)', fontsize=12)
plt.ylabel('건망증 점수', fontsize=12)
plt.title(f'선형 회귀: y = {b:.2f} + {m:.2f}x', fontsize=14)
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

🎉 **직선이 데이터에 완벽하게 맞춰졌습니다!**

---

## 8️⃣ 전체 코드 요약

```python
import numpy as np
import matplotlib.pyplot as plt

# 1. 데이터 준비
x = np.array([0, 1, 2, 3, 4, 5, 6, 7])
y = np.array([1.86, 1.31, 0.62, 0.33, 0.09, -0.67, -1.23, -1.37])

# 2. 입력 행렬 X 구성 (y절편용 1열 추가)
X = np.column_stack([np.ones(len(x)), x])

# 3. 가중치 계산: w = X⁺y
w = np.dot(np.linalg.pinv(X), y)
b, m = w[0], w[1]

print(f"회귀 방정식: y = {b:.4f} + {m:.4f}x")

# 4. 시각화
plt.figure(figsize=(10, 6))
plt.scatter(x, y, s=100, c='blue', label='데이터')
plt.plot([x.min(), x.max()],
         [m*x.min()+b, m*x.max()+b],
         'r-', linewidth=2, label='회귀 직선')
plt.xlabel('x')
plt.ylabel('y')
plt.title(f'Linear Regression: y = {b:.2f} + {m:.2f}x')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

---

## 9️⃣ 더 많은 특성으로 확장하기

### 다중 선형 회귀

특성이 여러 개인 경우에도 동일한 원리가 적용됩니다:

$$
y = b + w_1 x_1 + w_2 x_2 + \dots + w_m x_m
$$

```python
# 예: 3개 특성 (침실 수, 학교 거리, 면적)
X = np.array([
    [1, 2, 5, 70],    # 1 = 절편용
    [1, 3, 3, 85],
    [1, 4, 8, 120],
    [1, 2, 2, 65],
    # ... 더 많은 데이터
])

y = np.array([300, 400, 280, 320])  # 집값 (백만원)

# 가중치 계산
w = np.linalg.pinv(X) @ y
print(f"가중치: {w}")
# 결과: [절편, 침실_가중치, 학교_가중치, 면적_가중치]
```

---

## 🎯 요약

### 핵심 공식

$$
\mathbf{w} = X^+ \mathbf{y}
$$

| 단계 | 작업                                                    |
| :--- | :------------------------------------------------------ |
| 1    | 데이터 준비 ($x$, $y$)                                  |
| 2    | 입력 행렬 $X$ 구성 (1열 추가로 y절편 포함)              |
| 3    | 유사역행렬로 가중치 계산: $\mathbf{w} = X^+ \mathbf{y}$ |
| 4    | 회귀 직선 그리기: $y = b + mx$                          |

### 왜 이것이 놀라운가?

1. **비정방행렬도 OK**: 데이터가 아무리 많아도 해결 가능
2. **최적의 해**: 최소제곱 오차를 자동으로 최소화
3. **간단한 구현**: `np.linalg.pinv(X) @ y` 한 줄!

### 머신러닝에서의 의미

- **선형 회귀**의 수학적 기초
- **경사하강법 없이** 해석적으로 최적 가중치 계산
- **Normal Equation**이라고도 불림

---

## 다음 단계

다음은 선형대수학의 마지막 개념인 **대각합(Trace)**을 배웁니다. 이것으로 선형대수학 전체 이론이 완성됩니다!
