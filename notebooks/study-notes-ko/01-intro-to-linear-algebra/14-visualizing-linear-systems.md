# 14. 선형 연립방정식 시각화 (Visualization)

> Segment 2 마무리 - 해를 그래프로 확인하기

## 📌 개요

종이와 연필로 풀어본 선형 연립방정식의 해를 **그래프로 시각화**하여 직관적으로 이해해봅시다.

핵심: **두 직선의 교차점 = 연립방정식의 해**

---

## 🔧 시각화 준비

### 방정식을 y = f(x) 형태로 변환

matplotlib으로 그래프를 그리려면 **y를 x의 함수**로 표현해야 합니다.

```python
# 예: y = 3x (이미 완성된 형태)
# 예: -5x + 2y = 2 → y = (2 + 5x) / 2 = 1 + 5x/2
```

---

## 📊 예제 1: 치환법 문제

### 원래 연립방정식

$$
\begin{cases}
y = 3x \\
-5x + 2y = 2
\end{cases}
$$

### 함수 형태로 변환

$$
y_1 = 3x
$$

$$
y_2 = 1 + \frac{5x}{2}
$$

### Python 코드

```python
import numpy as np
import matplotlib.pyplot as plt

# x 범위 설정 (해가 (2, 6)이므로 -10 ~ 10 정도면 충분)
x = np.linspace(-10, 10, 1000)

# 두 직선의 방정식
y1 = 3 * x          # 첫 번째 식
y2 = 1 + (5*x) / 2  # 두 번째 식

# 그래프 그리기
fig, ax = plt.subplots()
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_xlim([-1, 3])
ax.set_ylim([0, 8])

ax.plot(x, y1, c='green')   # 첫 번째 직선
ax.plot(x, y2, c='brown')   # 두 번째 직선

# 교차점 표시 (x=2, y=6)
plt.axvline(x=2, color='purple', linestyle='--')
plt.axhline(y=6, color='purple', linestyle='--')

plt.show()
```

### 결과 해석

```
  y
  8│         /
   │        / ← y = 3x (녹색)
  6│- - - -●- - - - ← 교차점 (2, 6)
   │      /╱
  4│     /╱
   │    /╱ ← y = 1 + 5x/2 (갈색)
  2│   /╱
   │  /
  0└──────────→ x
      0  2
```

**교차점 (2, 6)** = 연립방정식의 해!

---

## 📊 예제 2: 소거법 문제

### 원래 연립방정식

$$
\begin{cases}
2x - 3y = 15 \\
4x + 10y = 14
\end{cases}
$$

### 함수 형태로 변환

**첫 번째 식:**

$$
-3y = 15 - 2x
$$

$$
y = \frac{15 - 2x}{-3} = -5 + \frac{2x}{3}
$$

**두 번째 식:**

$$
4x + 10y = 14 \xrightarrow{\div 2} 2x + 5y = 7
$$

$$
5y = 7 - 2x
$$

$$
y = \frac{7 - 2x}{5}
$$

### Python 코드

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-2, 10, 1000)

y1 = -5 + (2*x) / 3      # 첫 번째 식
y2 = (7 - 2*x) / 5       # 두 번째 식

fig, ax = plt.subplots()
ax.set_xlabel('x')
ax.set_ylabel('y')

# x, y 축 표시 (연회색)
plt.axhline(y=0, color='lightgray')
plt.axvline(x=0, color='lightgray')

ax.plot(x, y1, c='green')
ax.plot(x, y2, c='brown')

# 교차점 표시 (x=6, y=-1)
plt.axvline(x=6, color='purple', linestyle='--')
plt.axhline(y=-1, color='purple', linestyle='--')

plt.show()
```

### 결과 해석

```
  y
  2│    ╲
   │     ╲ ← y = (7-2x)/5 (갈색)
  0│──────╲─────────────→ x
   │       ╲    /
 -1│- - - - ●  / ← 교차점 (6, -1)
   │         ╳
 -3│        / ╲
   │       /   ← y = -5 + 2x/3 (녹색)
 -5│      /
      0  6
```

**교차점 (6, -1)** = 연립방정식의 해!

---

## 💡 차원과 시각화

| 차원 | 변수          | 시각화           |
| ---- | ------------- | ---------------- |
| 2D   | x, y          | 평면 그래프 ✓    |
| 3D   | x, y, z       | 3차원 그래프 ✓   |
| 4D+  | x, y, z, w... | 시각화 어려움 ❌ |

> 고차원에서도 **교차점 = 해**라는 개념은 동일!

---

## 🎯 핵심 정리

1. 방정식을 **y = f(x)** 형태로 변환
2. `np.linspace()`로 x 범위 설정
3. matplotlib으로 직선 그리기
4. **교차점 = 연립방정식의 해**
5. 축을 명확히 표시하면 음수 좌표도 이해하기 쉬움

---

## 🎉 Segment 2 완전 종료!

이제 **Segment 3: 행렬 특성 (Matrix Properties)** 을 배울 준비가 되었습니다!
