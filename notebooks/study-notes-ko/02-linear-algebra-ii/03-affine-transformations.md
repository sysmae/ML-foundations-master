# 3. 아핀 변환 (Affine Transformations)

> Segment 2: 고유값 분해 - 행렬 곱셈을 통한 기하학적 변환

## 📌 개요

이번 장에서는 **NumPy 코드 예제**를 통해 **아핀 변환(Affine Transformation)**의 작동 원리를 알아봅니다. 아핀 변환은 행렬을 사용하여 벡터를 뒤집거나(Reflection), 회전(Rotation), 크기 변환(Scaling)하는 등의 기하학적 변형을 수행하는 것입니다.

---

## 🛠️ 실습 준비

모든 코드는 제공된 Jupyter Notebook에서 직접 실행해볼 수 있습니다.

> [!TIP]
> Google Colab을 활용하여 직접 코드를 실행하고 그래프를 그려보며 학습하는 것을 권장합니다.

### 벡터 시각화 함수: `plot_vectors`

먼저 벡터를 2차원 평면에 시각화하기 위한 함수를 정의합니다. (기반: Hadrien Jean의 `plotVectors`)

```python
import matplotlib.pyplot as plt
import numpy as np

def plot_vectors(vectors, colors):
    """
    하나 이상의 벡터를 2D 평면에 화살표로 그립니다.
    """
    plt.figure()
    plt.axvline(x=0, color='lightgray')
    plt.axhline(y=0, color='lightgray')

    for i in range(len(vectors)):
        x = np.concatenate([[0,0], vectors[i]])
        plt.quiver([x[0]], [x[1]], [x[2]], [x[3]],
                   angles='xy', scale_units='xy', scale=1, color=colors[i])
```

---

## 1️⃣ 기본 벡터와 단위 행렬

어떤 벡터 $\mathbf{v}$가 좌표 $(3, 1)$에 있다고 가정해봅시다.

```python
v = np.array([3, 1])
plot_vectors([v], ['lightblue'])
plt.xlim(-1, 5)
plt.ylim(-1, 5)
```

### 단위 행렬 (Identity Matrix) 적용

**단위 행렬($I$)**을 벡터에 적용(곱셈)하면, 벡터는 변하지 않습니다.

```python
I = np.array([[1, 0], [0, 1]])
Iv = np.dot(I, v)
print(Iv)  # [3 1]
```

---

## 2️⃣ 반전 (Reflection): 행렬 E와 F

아핀 변환의 대표적인 예로 **축을 기준으로 한 반전(Flipping)**이 있습니다.

### X축 기준 반전 (Matrix E)

행렬 $E$는 벡터를 **x축 너머로** 뒤집습니다.

$$
E = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}
$$

```python
E = np.array([[1, 0], [0, -1]])
Ev = np.dot(E, v)
print(Ev)  # [3 -1]
```

- y좌표의 부호가 반대로 바뀝니다 ($1 \to -1$).
- 시각화해 보면 x축을 기준으로 대칭 이동한 것을 확인할 수 있습니다.

### Y축 기준 반전 (Matrix F)

행렬 $F$는 벡터를 **y축 너머로** 뒤집습니다.

$$
F = \begin{bmatrix} -1 & 0 \\ 0 & 1 \end{bmatrix}
$$

```python
F = np.array([[-1, 0], [0, 1]])
Fv = np.dot(F, v)
print(Fv)  # [-3 1]
```

- x좌표의 부호가 반대로 바뀝니다 ($3 \to -3$).

> [!NOTE] > **아핀 변환의 특징**: 아핀 변환은 벡터 간의 거리나 각도를 변화시킬 수 있지만, **평행성(Parallelism)**은 보존합니다. 즉, 평행한 두 벡터는 변환 후에도 여전히 평행합니다.

---

## 3️⃣ 복합 변환과 행렬 A

단순한 반전이나 회전뿐만 아니라, 행렬은 여러 변환을 동시에 수행할 수 있습니다.

다음 행렬 $A$를 살펴봅시다:

$$
A = \begin{bmatrix} -1 & 4 \\ 2 & -2 \end{bmatrix}
$$

```python
A = np.array([[-1, 4], [2, -2]])
Av = np.dot(A, v)
print(Av)
# [-1 4] 계산됨: (-1*3 + 4*1 = 1), (2*3 + -2*1 = 4) -> 실제 코드는 np.dot 결과 참고
```

행렬 $A$를 적용하면 벡터는 회전하고, 길이가 변하며(Scaling), 전단(Shearing) 변형이 일어날 수 있습니다.

---

## 4️⃣ 행렬에 행렬 적용: 다수 벡터 동시 변환

여러 개의 벡터를 한 번에 변환하려면, 벡터들을 연결하여 **행렬**로 만든 후 연산을 수행합니다.

### 4개의 벡터 정의

```python
v = np.array([3, 1])
v2 = np.array([2, 1])
v3 = np.array([-3, -1])
v4 = np.array([-1, 1])
```

### 행렬 V 생성 (열벡터 결합)

벡터들을 열(column)로 갖는 행렬 $V$를 생성합니다.

```python
# 벡터를 2D 배열(열벡터)로 변환 후 가로 방향(axis=1)으로 연결
V = np.concatenate((np.matrix(v).T,
                    np.matrix(v2).T,
                    np.matrix(v3).T,
                    np.matrix(v4).T),
                   axis=1)
print(V)
```

### 선형 변환 적용

행렬 $A$를 행렬 $V$에 곱하면($AV$), $V$의 **모든 열벡터에 대해 동시다발적**으로 $A$에 의한 변환이 적용됩니다.

```python
AV = np.dot(A, V)
```

### 시각화를 위한 헬퍼 함수: `vectorfy`

행렬 형태의 결과에서 각 열을 다시 1차원 벡터로 변환하여 시각화하기 쉽게 만드는 함수입니다.

```python
def vectorfy(mtrx, clmn):
    return np.array(mtrx[:, clmn]).reshape(-1)
```

### 변환 전후 시각화

```python
# 변환 전 (밝은 색): v, v2, v3, v4
# 변환 후 (어두운 색): Av, Av2, Av3, Av4

plot_vectors([vectorfy(V, 0), vectorfy(V, 1), vectorfy(V, 2), vectorfy(V, 3),
              vectorfy(AV, 0), vectorfy(AV, 1), vectorfy(AV, 2), vectorfy(AV, 3)],
             ['lightblue', 'lightgreen', 'lightgray', 'orange',
              'blue', 'green', 'gray', 'red'])

plt.xlim(-4, 6)
plt.ylim(-5, 5)
```

변환 결과를 보면:

- 모든 벡터가 행렬 $A$에 의해 **회전**되고 **길이가 변화**된 것을 확인할 수 있습니다.
- 벡터의 위치에 따라 변환의 결과가 극적으로 다르게 나타날 수 있습니다.

---

## 🎯 결론

- **행렬 적용(Matrix Application)**은 벡터에 대한 **기하학적 선형 변환(Linear Transformation)**입니다.
- **아핀 변환**에는 반전, 회전, 크기 변환, 전단 등이 포함됩니다.
- 행렬 곱셈을 통해 **여러 벡터를 동시에 변환**할 수 있습니다.

다음 영상에서는 **고유벡터(Eigenvector)와 고유값(Eigenvalue)** 이론을 본격적으로 다뤄보겠습니다.
