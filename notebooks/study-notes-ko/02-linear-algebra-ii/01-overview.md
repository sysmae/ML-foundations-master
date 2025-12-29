# 1. 개요 (Overview)

> **"선형대수학 II: 행렬 연산"**에 오신 것을 환영합니다!

이 섹션은 머신러닝, 특히 **딥러닝**의 핵심이 되는 텐서 조작(Tensor Manipulation)을 심도 있게 다룹니다.

---

## 🎯 학습 목표

이 과정을 통해 여러분은 다음을 달성하게 됩니다:

1.  **기하학적 직관 개발**: 머신러닝 알고리즘 내부에서 일어나는 일을 기하학적으로 이해합니다.
2.  **ML 논문 이해**: 논문이나 고급 교재에 나오는 수식과 개념을 파악할 수 있는 능력을 기릅니다.
3.  **차원 축소 마스터**: 고차원의 복잡한 데이터를 가장 중요한 정보만 남기고 압축하는 기술(SVD, PCA 등)을 배웁니다.

---

## 📚 커리큘럼 구성

이 노트 시리즈는 크게 세 가지 세그먼트로 나뉩니다:

### Segment 1: 기초 선형대수 복습 (Review)

기초가 튼튼해야 멀리 갑니다. 1부 내용을 빠르게 복습하고 심화합니다.

- 벡터 전치(Transposition)와 노름(Norm)
- 행렬 곱셈(Matrix Multiplication)과 역행렬(Inversion)
- 특수 행렬: 단위 행렬, 대각 행렬, 직교 행렬

### Segment 2: 고유값 분해 (Eigendecomposition)

행렬이 공간을 어떻게 변형시키는지, 그 본질을 파악합니다.

- **아핀 변환(Affine Transformation)**: 회전, 크기 변환, 전단 등
- **고유벡터(Eigenvectors)와 고유값(Eigenvalues)**
- 행렬식(Determinants)

### Segment 3: 머신러닝을 위한 행렬 연산 (ML Applications)

실전 머신러닝에서 쓰이는 강력한 도구들을 다룹니다.

- **특이값 분해(SVD)**: 모든 행렬을 분해하는 마법의 키
- **무어-펜로즈 의사역행렬(Moore-Penrose Pseudoinverse)**: 해가 없는 방정식의 해를 구하기
- **주성분 분석(PCA)**: 데이터 차원 축소의 정석

---

## 🛠️ 실습 환경

모든 예제 코드는 **Python (NumPy, PyTorch)**을 사용하여 직접 실행해볼 수 있습니다.

- [원본 노트북 보기 (GitHub)](https://github.com/jonkrohn/ML-foundations/blob/master/notebooks/2-linear-algebra-ii.ipynb)
- [Google Colab에서 실행하기](https://colab.research.google.com/github/jonkrohn/ML-foundations/blob/master/notebooks/2-linear-algebra-ii.ipynb)

> [!TIP]
> 눈으로만 보지 말고, 직접 코드를 치며 실행해보세요!
