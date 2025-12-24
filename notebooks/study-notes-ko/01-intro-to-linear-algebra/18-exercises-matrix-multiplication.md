# 18. 연습문제: 행렬 곱셈 📝

> Segment 3: 행렬 특성 - 복습 및 실습

## 🎯 목표

행렬 곱셈과 단위 행렬의 개념을 직접 풀어보며 확인!

---

## 📝 문제 1: 행렬 × 벡터

### 문제

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 & 2 \\ 3 & 4 & 5 \\ 6 & 7 & 8 \end{bmatrix}, \quad
\mathbf{b} = \begin{bmatrix} -1 \\ 1 \\ -2 \end{bmatrix}
$$

$\mathbf{Ab}$를 구하세요.

### 풀이

**조건 확인**: A(3×3) × b(3×1) → 결과: 3×1 ✓

**계산**:

$$
\begin{bmatrix}
0 \cdot (-1) + 1 \cdot 1 + 2 \cdot (-2) \\
3 \cdot (-1) + 4 \cdot 1 + 5 \cdot (-2) \\
6 \cdot (-1) + 7 \cdot 1 + 8 \cdot (-2)
\end{bmatrix}
= \begin{bmatrix}
0 + 1 - 4 \\
-3 + 4 - 10 \\
-6 + 7 - 16
\end{bmatrix}
= \begin{bmatrix} -3 \\ -9 \\ -15 \end{bmatrix}
$$

### 정답

$$
\mathbf{Ab} = \begin{bmatrix} -3 \\ -9 \\ -15 \end{bmatrix}
$$

---

## 📝 문제 2: 단위 행렬 × 벡터

### 문제

$$
\mathbf{I}_3 = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}, \quad
\mathbf{b} = \begin{bmatrix} -1 \\ 1 \\ -2 \end{bmatrix}
$$

$\mathbf{I}_3 \mathbf{b}$를 구하세요.

### 풀이

**계산**:

$$
\begin{bmatrix}
1 \cdot (-1) + 0 \cdot 1 + 0 \cdot (-2) \\
0 \cdot (-1) + 1 \cdot 1 + 0 \cdot (-2) \\
0 \cdot (-1) + 0 \cdot 1 + 1 \cdot (-2)
\end{bmatrix}
= \begin{bmatrix} -1 + 0 + 0 \\ 0 + 1 + 0 \\ 0 + 0 - 2 \end{bmatrix}
= \begin{bmatrix} -1 \\ 1 \\ -2 \end{bmatrix}
$$

### 정답

$$
\mathbf{I}_3 \mathbf{b} = \begin{bmatrix} -1 \\ 1 \\ -2 \end{bmatrix} = \mathbf{b}
$$

> ✨ **단위 행렬의 특성**: $\mathbf{Ix} = \mathbf{x}$ 확인!

---

## 📝 문제 3: 행렬 × 행렬

### 문제

$$
\mathbf{A} = \begin{bmatrix} 0 & 1 & 2 \\ 3 & 4 & 5 \\ 6 & 7 & 8 \end{bmatrix}, \quad
\mathbf{B} = \begin{bmatrix} -1 & 0 \\ 1 & 1 \\ -2 & 2 \end{bmatrix}
$$

$\mathbf{AB}$를 구하세요.

### 풀이

**조건 확인**: A(3×3) × B(3×2) → 결과: 3×2 ✓

**첫 번째 열** (B의 [-1, 1, -2] 사용):

문제 1과 동일! → $\begin{bmatrix} -3 \\ -9 \\ -15 \end{bmatrix}$

**두 번째 열** (B의 [0, 1, 2] 사용):

$$
\begin{bmatrix}
0 \cdot 0 + 1 \cdot 1 + 2 \cdot 2 \\
3 \cdot 0 + 4 \cdot 1 + 5 \cdot 2 \\
6 \cdot 0 + 7 \cdot 1 + 8 \cdot 2
\end{bmatrix}
= \begin{bmatrix} 0 + 1 + 4 \\ 0 + 4 + 10 \\ 0 + 7 + 16 \end{bmatrix}
= \begin{bmatrix} 5 \\ 14 \\ 23 \end{bmatrix}
$$

### 정답

$$
\mathbf{AB} = \begin{bmatrix} -3 & 5 \\ -9 & 14 \\ -15 & 23 \end{bmatrix}
$$

---

## 💡 핵심 정리

| 문제   | 핵심 포인트                     |
| ------ | ------------------------------- |
| 문제 1 | 행렬 × 벡터 기본 계산           |
| 문제 2 | $\mathbf{Ix} = \mathbf{x}$ 확인 |
| 문제 3 | 행렬 × 행렬 = 벡터 곱셈 반복    |

> 💡 **팁**: 행렬 × 행렬은 "왼쪽 행렬 × 오른쪽 각 열 벡터"를 반복하는 것!

---

## 🎯 다음 주제

연습을 마쳤다면, 다음으로 **역행렬 (Matrix Inversion)** 을 배워봅시다!
