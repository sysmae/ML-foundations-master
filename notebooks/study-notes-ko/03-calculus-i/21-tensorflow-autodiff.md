# 21. TensorFlow 자동 미분 (Autodiff with TensorFlow)

> **"GradientTape를 이용한 미분 기록"**

이번 장에서는 구글의 **TensorFlow** 라이브러리를 사용하여 미분을 계산해 봅니다.
PyTorch의 직관적인 방식(`x.grad`)과는 다르게, TensorFlow는 `GradientTape`라는 문맥(Context)을 사용하여 연산을 기록합니다.

---

## 1️⃣ TensorFlow 구현 방식

TensorFlow에서 자동 미분을 수행하는 과정은 다음과 같습니다.

### 단계 1: 변수(Variable) 생성

```python
import tensorflow as tf

# x = 5.0 생성
# tf.Variable은 값을 변경할 수 있는 텐서입니다.
x = tf.Variable(5.0)

print(x)
# <tf.Variable 'Variable:0' shape=() dtype=float32, numpy=5.0>
```

### 단계 2: GradientTape로 연산 기록

`tf.GradientTape()` 컨텍스트 안에서 수행되는 연산만이 미분을 위해 기록됩니다.

```python
with tf.GradientTape() as t:
    # 1. 미분할 변수를 감시(watch)합니다.
    # (tf.Variable은 자동으로 감시되지만, 상수 텐서는 t.watch(x)가 필요할 수 있습니다.)
    t.watch(x)

    # 2. Forward Pass (함수 계산)
    y = x**2

print(y)
# tf.Tensor(25.0, shape=(), dtype=float32)
```

### 단계 3: 기울기 계산 (Get Gradient)

`t.gradient(target, sources)` 메서드를 사용하여 미분값을 구합니다.

```python
# y를 x에 대해 미분 (dy/dx)
dy_dx = t.gradient(y, x)

print(dy_dx)
# tf.Tensor(10.0, shape=(), dtype=float32)
```

역시 정답인 **10**이 출력됩니다.

---

## 2️⃣ PyTorch vs TensorFlow 비교

| 특징          | PyTorch                            | TensorFlow                                                          |
| :------------ | :--------------------------------- | :------------------------------------------------------------------ |
| **방식**      | **Define-by-Run** (동적 그래프)    | **Define-and-Run** (정적 그래프, 과거), 현재는 Eager Execution 지원 |
| **미분 설정** | `requires_grad=True` 속성 설정     | `with tf.GradientTape() as t:` 블록 사용                            |
| **직관성**    | 파이썬 코드와 유사하여 매우 직관적 | 다소 복잡하고 독특한 문법 (`Tape` 개념)                             |

> **강사의 의견**: "PyTorch가 훨씬 직관적이고 코드를 작성하는 재미가 있습니다. 따라서 이 강의의 나머지 머신러닝 예제들은 주로 PyTorch를 사용할 것입니다."

---

## 🎯 요약

- **TensorFlow**에서는 `tf.GradientTape`를 사용하여 연산을 기록(Record)해야 미분이 가능합니다.
- `t.gradient(y, x)`를 호출하여 미분값을 얻습니다.
- 두 라이브러리 모두 자동 미분을 강력하게 지원하지만, 사용 패턴에는 차이가 있습니다.

다음 장부터는 이 자동 미분 기술을 활용하여 간단한 **머신러닝 알고리즘(회귀 분석)**을 직접 구현해 보겠습니다!
