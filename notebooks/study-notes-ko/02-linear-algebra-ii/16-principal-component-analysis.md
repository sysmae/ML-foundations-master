# 16. 주성분 분석 (Principal Component Analysis, PCA)

> **"복잡한 데이터를 핵심만 남겨 시각화하다"**

이번 장에서는 선형대수학의 모든 개념(고유값 분해, SVD, 대각합 등)을 집대성한 머신러닝 알고리즘, **주성분 분석(PCA)**을 실습합니다.

PCA는 **비지도 학습(Unsupervised Learning)**의 대표 주자로, 라벨이 없는 데이터의 숨겨진 구조를 찾아내고 차원을 축소하는 강력한 도구입니다.

---

## 1️⃣ PCA란 무엇인가?

### 핵심 아이디어

고차원 데이터(예: 4차원)를 저차원(예: 2차원)으로 압축하되, **데이터의 분산(변화량)을 최대한 보존**하는 축을 찾습니다.

- **제1주성분 (PC1)**: 데이터의 구조(분산)를 가장 많이 설명하는 축
- **제2주성분 (PC2)**: PC1과 직교하면서 그 다음으로 많은 분산을 설명하는 축

### 활용 분야

1. **차원 축소**: 수천 개의 특성을 몇 개의 주성분으로 압축
2. **데이터 시각화**: 4차원 이상의 데이터를 2차원 평면에 그려 구조 파악
3. **노이즈 제거**: 덜 중요한 주성분을 제거하여 신호 강화

---

## 2️⃣ 실습: 붓꽃(Iris) 데이터셋 분석

붓꽃 데이터셋은 머신러닝의 "Hello World"와 같습니다.

| 데이터        | 개수  | 특성 (4개)                                     | 설명                                       |
| :------------ | :---- | :--------------------------------------------- | :----------------------------------------- |
| **붓꽃 샘플** | 150개 | 꽃받침 길이, 꽃받침 너비, 꽃잎 길이, 꽃잎 너비 | 3가지 품종 (Setosa, Versicolor, Virginica) |

### 1단계: 데이터 불러오기

```python
import pandas as pd
from sklearn import datasets

# 붓꽃 데이터셋 로드
iris = datasets.load_iris()

# 데이터 확인 (처음 5개 행)
# 특성: sepal length, sepal width, petal length, petal width
print("특성 이름:", iris.feature_names)
print("데이터 shape:", iris.data.shape)  # (150, 4)
print("\n첫 5개 샘플:\n", iris.data[:5])
```

### 2단계: PCA 적용 (4차원 → 2차원)

우리는 4개의 특성을 가졌지만, 4차원 공간을 시각화할 수는 없습니다. PCA를 이용해 가장 중요한 2개의 축으로 압축해 봅시다.

```python
from sklearn.decomposition import PCA

# PCA 모델 생성 (2개의 주성분만 남김)
pca = PCA(n_components=2)

# 데이터에 적합 및 변환 (Fit & Transform)
X_pca = pca.fit_transform(iris.data)

print("변환된 데이터 shape:", X_pca.shape)  # (150, 2)
print("\n변환된 첫 5개 샘플 (PC1, PC2):\n", X_pca[:5])
```

**해석**:

- 원래 4개의 숫자로 표현되던 꽃이 이제 **단 2개의 숫자(좌표)**로 표현됩니다.
- 이 2개의 숫자가 원본 정보의 대부분을 담고 있습니다.

### 3단계: 시각화 (데이터 구조 파악)

이제 2차원으로 축소된 데이터를 산점도로 그려봅시다.

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(8, 6))

# 모든 데이터를 점으로 찍기
plt.scatter(X_pca[:, 0], X_pca[:, 1], c='gray', edgecolor='k', alpha=0.7)

plt.xlabel('제1 주성분 (PC1)')
plt.ylabel('제2 주성분 (PC2)')
plt.title('붓꽃 데이터의 PCA 시각화 (라벨 미포함)')
plt.grid(True, alpha=0.3)
plt.show()
```

**관찰**:

- 데이터가 **두 개의 덩어리**로 나누어져 있는 것이 보입니다.
- 왼쪽의 작은 덩어리와 오른쪽의 큰 덩어리가 구분됩니다.
- 이것이 라벨 없이도 발견한 **데이터의 숨겨진 구조**입니다!

### 4단계: 정답(라벨)과 비교

사실 붓꽃 데이터에는 품종 라벨이 있습니다. 우리가 발견한 구조가 실제 품종과 일치하는지 확인해 봅시다.

```python
# 품종별로 색깔 다르게 표시
plt.figure(figsize=(8, 6))

scatter = plt.scatter(X_pca[:, 0], X_pca[:, 1],
                      c=iris.target,      # 품종에 따라 색상 매핑
                      cmap='viridis',
                      edgecolor='k',
                      alpha=0.7)

# 범례 추가
plt.legend(handles=scatter.legend_elements()[0],
           labels=iris.target_names.tolist(),
           title="품종")

plt.xlabel('제1 주성분 (PC1)')
plt.ylabel('제2 주성분 (PC2)')
plt.title('붓꽃 데이터의 PCA 시각화 (품종 포함)')
plt.grid(True, alpha=0.3)
plt.show()
```

**결과**:

- **Setosa (보라색)**: 완벽하게 분리되어 있습니다. (왼쪽 덩어리)
- **Versicolor (청록색) & Virginica (노란색)**: 약간 겹치지만 대체로 잘 구분됩니다. (오른쪽 덩어리)

**결론**: PCA는 라벨 정보를 전혀 사용하지 않았음에도, 데이터의 **내재된 패턴(품종 차이)**을 성공적으로 시각화했습니다.

---

## 3️⃣ PCA와 선형대수학의 연결

PCA 알고리즘 내부에서는 우리가 배웠던 선형대수학 개념들이 작동하고 있습니다:

1. **데이터 중심화**: 평균을 0으로 맞춤
2. **공분산 행렬 계산**: 데이터의 상관관계 파악
3. **고유값 분해 (또는 SVD)**:
   - **고유벡터** = 주성분 (데이터가 가장 많이 퍼진 방향)
   - **고유값** = 해당 주성분의 분산 크기 (중요도)
4. **투영**: 데이터를 주성분 축으로 회전 및 차원 축소

(더 깊은 수학적 원리는 Ian Goodfellow의 **Deep Learning Book 2.12장**을 참고하세요!)

---

## 4️⃣ 전체 코드 요약

```python
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.decomposition import PCA

# 1. 데이터 로드
iris = datasets.load_iris()
X = iris.data
y = iris.target
target_names = iris.target_names

# 2. PCA 적용 (4차원 -> 2차원)
pca = PCA(n_components=2)
X_r = pca.fit_transform(X)

# 3. 시각화
plt.figure(figsize=(10, 8))
colors = ['navy', 'turquoise', 'darkorange']
lw = 2

for color, i, target_name in zip(colors, [0, 1, 2], target_names):
    plt.scatter(X_r[y == i, 0], X_r[y == i, 1], color=color, alpha=.8, lw=lw,
                label=target_name)

plt.legend(loc='best', shadow=False, scatterpoints=1)
plt.title('PCA of IRIS dataset')
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.show()

# 4. 분산 설명 비율 확인
print(f"설명된 분산 비율: {pca.explained_variance_ratio_}")
# 출력 예: [0.9246, 0.0530] -> PC1이 92%, PC2가 5% 설명함 (총 97% 보존!)
```

---

## 🎯 선형대수학 II 대단원 마무리

축하합니다! 🎉

이것으로 **선형대수학 II: 행렬 연산**의 모든 여정이 끝났습니다.

1. **Review**: 벡터, 노름, 행렬 연산 복습
2. **Eigendecomposition**: 고유값, 고유벡터, 행렬식, 아핀 변형
3. **Matrix Operations**: SVD, 무어-펜로즈 역행렬, 대각합, PCA

이제 여러분은 머신러닝 논문을 읽고 알고리즘을 이해하는 데 필요한 **탄탄한 수학적 기초**를 갖추었습니다. 다음 단계인 **미적분학(Calculus)**에서도 이 기초가 큰 힘이 될 것입니다!

수고하셨습니다! 👏👏👏
