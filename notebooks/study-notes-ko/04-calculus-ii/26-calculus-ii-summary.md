# 26. 미적분학 II 요약 및 마무리 (Calculus II Summary & Wrap-up)

> **"미적분의 여정을 마치며"**

축하합니다! 🎉
이로써 **"Calculus II: Partial Derivatives & Integrals"** 모듈을 모두 마쳤습니다.
이 과정은 머신러닝 기초 커리큘럼(총 8과목)의 **절반**에 해당합니다. (4/8 완료)

---

## 1️⃣ 학습 내용 요약

이번 모듈은 크게 3가지 세그먼트로 진행되었습니다.

### A. 편미분 (Partial Derivatives)

- **개념**: 변수가 여러 개인 함수에서 하나만 변할 때의 기울기.
- **연쇄 법칙 (Chain Rule)**: 여러 함수가 중첩된 딥러닝 모델에서 미분하는 핵심 원리.
- **역전파 (Backpropagation)**: 연쇄 법칙의 머신러닝 버전. 비용 함수의 오차를 입력 쪽으로 전파합니다.

### B. 경사 (Gradients)

- **$\nabla C$ (Gradient Vector)**: 모든 파라미터에 대한 편미분을 모은 벡터.
- **경사 하강법 (Gradient Descent)**: 경사의 반대 방향으로 이동하며 오차를 줄이는 최적화 알고리즘.
- **수기 유도 vs 자동 미분**: 직접 손으로 미분해보고, PyTorch가 이를 어떻게 자동화하는지 확인했습니다.

### C. 적분 (Integrals)

- **개념**: 미분의 역연산. 곡선 아래의 면적을 구합니다.
- **ROC AUC**: 이진 분류 모델의 성능을 평가하기 위해 적분을 사용했습니다.
- **계산**: 기호적 방법(손 계산)과 수치적 방법(Python `quad`, `auc`)을 모두 익혔습니다.

---

## 2️⃣ 추천 자료 (Resources)

더 깊이 공부하고 싶은 분들을 위해 Jon Krohn 강사님이 추천하는 자료입니다.

- **미분학 (Calculus)**:
  - 📖 _Mathematics for Machine Learning_ by Deisenroth et al. (Chapter 5)
  - 📺 **3Blue1Brown** 유튜브 채널 (Calculus 시리즈)
- **적분학 (Integrals)**:
  - 📖 _Dive into Deep Learning_ by Zhang et al. (Appendix 18.5)

---

## 3️⃣ 다음 단계 (What's Next?)

이제 우리는 미분과 적분이라는 강력한 수학적 도구를 갖췄습니다.
다음 과목은 **"5. 확률과 정보 이론 (Probability & Information Theory)"**입니다.

- **왜 확률인가?**: 머신러닝 모델은 본질적으로 불확실성(Uncertainty)을 다룸.
- **기대**: 베이즈 정리, 엔트로피 등 데이터 사이언스의 핵심 개념들을 배우게 됩니다.

새로운 여정에서 만나요! 👋
