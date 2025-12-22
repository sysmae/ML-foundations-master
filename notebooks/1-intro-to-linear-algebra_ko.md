# Intro to Linear Algebra — 한국어 번역 및 설명

이 문서는 노트북 `1-intro-to-linear-algebra.ipynb`의 모든 마크다운 셀 내용을 한국어로 번역하고 간단한 학습 설명을 덧붙인 것입니다. 코드 셀과 출력은 포함되지 않습니다. 원본 슬라이드/노트와 함께 보면서 참고하세요.

---

# 학습 요약 (한국어)

**이 노트북의 핵심 개념**: 벡터와 행렬, 선형 연산(덧셈·스칼라 곱), 내적, 선형 변환 및 시각화 예제.

**학습 팁**: 수학적 정의를 먼저 읽고, 바로 아래의 코드 셀을 실행해 결과와 그림을 확인하세요. 단계별로 입력값을 바꿔가며 직관을 익히면 이해에 도움이 됩니다.

---

> Original: Open In Colab badge link

Colab에서 열기 버튼(원본 노트북 링크).

---

# Intro to Linear Algebra

선형대수학 입문

---

이 주제, "Intro to Linear Algebra"는 "Machine Learning Foundations" 시리즈의 첫 번째 항목입니다.

선형대수는 대부분의 머신러닝 방법의 핵심이며, 특히 딥러닝에서 매우 중요합니다. 이 노트북은 이론 설명과 상호작용 예제를 결합하여, 고차원 공간에서 미지수를 푸는 방법과 머신이 패턴을 인식하고 예측하는 방식에 대한 이해를 돕습니다.

이 노트북의 내용은 시리즈의 다른 모든 주제의 기초이며, 특히 "Linear Algebra II"와 밀접한 관련이 있습니다.

---

학습을 통해 달성할 목표:

- 선형대수의 기본 원리 이해
- 머신러닝 알고리즘 내부의 기하학적 직관 개발
- 머신러닝 관련 논문과 미적분·통계·최적화 등 다른 기초 과목의 세부사항을 보다 잘 이해할 수 있게 됨

---

**참고**: 이 Jupyter 노트북은 단독으로 사용하기보다 Jon Krohn의 "Machine Learning Foundations" 강의 또는 동영상의 보조 코드입니다. 강의에서 다음 항목들을 다룹니다:

- 섹션 1: 대수용 데이터 구조 (Tensors, Scalars, Vectors 등)
- 섹션 2: 일반적인 텐서 연산(전치, 산술, 축소, 내적 등)
- 섹션 3: 행렬의 성질(노름, 곱셈, 대각행렬, 역행렬 등)

(원문에 있는 항목 목록을 요약하여 제시)

---

## Segment 1: Data Structures for Algebra

섹션 1: 대수를 위한 데이터 구조

*슬라이드를 시작할 때 사용한 내용으로, 선형대수가 무엇인지 소개하고 간단한 손 계산 연습을 포함합니다.*

### What Linear Algebra Is

선형대수란 무엇인가

---

Distance travelled by robber: $d = 2.5t$

도둑이 이동한 거리: $d = 2.5t$

---

Distance travelled by sheriff: $d = 3(t-5)$

보안관이 이동한 거리: $d = 3(t-5)$

---

**설명 (한국어)**: 아래의 시각화는 벡터 연산과 관련된 그래프나 계산 예제입니다.

- 벡터 합: 기하학적 덧셈(꼭짓점을 이어 평행이동)으로 결과 벡터를 확인하세요.
- 내적: 두 벡터의 유사도를 나타내며 코사인 각도와 연결됩니다.
- 실습 포인트: 입력 벡터 값을 바꿔 그래프와 수치가 어떻게 변하는지 관찰하세요.

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### Scalars (Rank 0 Tensors) in Base Python

스칼라(랭크 0 텐서) — 기본 Python에서의 예

---

### Scalars in PyTorch

PyTorch에서의 스칼라 설명: PyTorch와 TensorFlow는 자동 미분을 지원하는 주요 라이브러리입니다. PyTorch 텐서는 NumPy와 비슷한 사용감을 제공하며, GPU 연산 지원이 장점입니다.

---

### Scalars in TensorFlow (version 2.0 or later)

TensorFlow(버전 2.0 이상)에서의 스칼라

Tensor 생성 방법(`tf.Variable`, `tf.constant` 등) 및 데이터 타입 관련 참고.

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### Vectors (Rank 1 Tensors) in NumPy

벡터(랭크 1 텐서) — NumPy 예제

---

### Vector Transposition

벡터 전치: 1차원 배열의 전치는 효과가 없지만, 행렬 스타일로 표현하면 전치 결과가 달라집니다(행→열 변환).

---

### Zero Vectors

영벡터(제로 벡터): 다른 벡터에 더해도 영향이 없습니다.

---

### Vectors in PyTorch and TensorFlow

PyTorch와 TensorFlow에서의 벡터 사용 예

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### $L^2$ Norm

$L^2$ 노름(유클리디안 노름): 벡터의 길이(예: 3차원 벡터의 경우 제곱합의 제곱근).

---

So, if units in this 3-dimensional vector space are meters, then the vector $x$ has a length of 25.6m

만약 이 3차원 벡터 공간의 단위가 미터라면, 벡터 $x$의 길이는 25.6m입니다.

---

### $L^1$ Norm

$L^1$ 노름: 각 성분의 절대값 합

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### Squared $L^2$ Norm

제곱된 $L^2$ 노름: 제곱합(내적과 연결되어 있음, np.dot(x,x) 참조)

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### Max Norm

최대 노름: 성분들의 절대값 중 최댓값

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### Orthogonal Vectors

직교 벡터(Orthogonal vectors): 내적이 0인 벡터쌍

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

### Matrices (Rank 2 Tensors) in NumPy

행렬(랭크 2 텐서) — NumPy에서의 예시와 슬라이스(인덱싱) 방법

---

### Matrices in PyTorch

PyTorch에서의 행렬 예시(NumPy와 유사하지만 파이토닉한 사용법)

---

### Matrices in TensorFlow

TensorFlow에서의 행렬 예시와 관련 함수(예: tf.rank, tf.shape)

---

**Return to slides here.**

(슬라이드로 돌아가기)

---

## Segment 2: Common Tensor Operations

섹션 2: 일반적인 텐서 연산

---

### Tensor Transposition

텐서 전치(전치 연산)

---

### Basic Arithmetical Properties

기본 산술 속성: 스칼라와의 덧셈/곱셈은 텐서의 각 요소에 적용되며 텐서의 형상은 유지됩니다. 이는 행렬 곱과 다르며, 같은 크기의 텐서끼리는 요소별 연산(Hadamard product)을 수행합니다.

---

### Reduction

축소 연산(Reduction): 텐서의 모든 요소 합, 최대/최소/평균/곱 등. 축을 지정해 특정 축으로만 합을 계산할 수 있습니다.

---

### The Dot Product

내적(점곱): 같은 길이의 두 벡터에 대해 대응 성분 곱의 합으로 계산되며 스칼라를 결과로 합니다. 딥러닝에서 인공신경망의 각 뉴런에서 광범위하게 사용됩니다.

---

### Solving Linear Systems

선형 연립방정식 풀기: 대입법(Substitution)과 소거법(Elimination) 예시를 통해 그래프와 해 찾기.

---

(두 가지 예 — Substitution 및 Elimination을 설명)

Substitution 예: 두 방정식
- $y = 3x$
- $-5x + 2y = 2$

두 번째 식을 정리해 $y$를 구하면 $y = 1 + \frac{5x}{2}$

Elimination 예: 두 방정식
- $2x - 3y = 15$
- $4x + 10y = 14$

각 식을 정리하면 교차점(해)을 그래프에서 시각적으로 확인할 수 있습니다.

---

## Segment 3: Matrix Properties

섹션 3: 행렬의 성질

---

### Frobenius Norm

프로베니우스 노름(Frobenius norm): 행렬의 모든 원소 제곱합의 제곱근(벡터 $L^2$ 노름의 행렬 버전).

---

### Matrix Multiplication (with a Vector)

행렬-벡터 곱: 행렬과 벡터의 곱(예: np.dot(A, b)) — 내적 연산을 일반화한 형태.

---

### Matrix Multiplication (with Two Matrices)

행렬 곱: 두 행렬의 곱은 일반적으로 교환적이지 않습니다(AB != BA). 차원 호환성에 주의하세요.

---

### Symmetric Matrices

대칭 행렬: 전치와 원래 행렬이 같은 행렬(X^T == X)

---

### Identity Matrices

단위 행렬(I): 곱셈의 항등원으로, I * x = x를 만족합니다.

---

### Answers to Matrix Multiplication Qs

행렬 곱 연습 문제의 해답(예시 코드를 통해 행렬 곱을 확인)

---

### Matrix Inversion

행렬의 역행렬: 역행렬 X^{-1}이 존재하면 X^{-1}X = I를 만족합니다. 정사각행렬이며 가역(비특이)일 때만 존재합니다.

---

(역행렬이 존재하지 않는 경우 — 특이행렬 및 비정방행렬 설명)

Matrix Inversion Where No Solution: 예시로 특이행렬을 보여주고 역행렬 계산 시 에러가 발생함을 설명합니다.

---

### Orthogonal Matrices

직교 행렬: 열벡터들이 상호 직교하며 단위 노름을 가질 때 행렬은 직교(orthogonal)입니다. 즉 A^T A = I를 만족합니다.

---

(Exercise 관련 설명: I_3의 열들이 상호 직교이고 단위 노름임을 보이는 단계 및 K 행렬에 대해 동일한 검증을 수행)

---

# 부록 및 연습

원문 노트의 "Exercises" 부분은 다음과 같습니다:

1. NumPy로 계산한 방법을 PyTorch로 반복하여 w를 계산하고 y = Xw가 성립함을 확인하세요.
2. 동일한 연습을 TensorFlow로도 수행하세요.

---

문서 끝. 원하시면 이 파일을 더 포맷하거나, 각 코드 셀 바로 아래에 해당 번역을 자동으로 삽입해드리겠습니다.
