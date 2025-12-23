# 2. 텐서와 스칼라

## 📊 텐서 계층

| Rank | 이름   | 예시             | Shape       |
| ---- | ------ | ---------------- | ----------- |
| 0    | 스칼라 | `25`             | `()`        |
| 1    | 벡터   | `[1, 2, 3]`      | `(3,)`      |
| 2    | 행렬   | `[[1,2], [3,4]]` | `(2, 2)`    |
| 3+   | 고차원 | 이미지, 비디오   | `(C, H, W)` |

---

## ✏️ 스칼라 (Rank 0)

단일 숫자. *이탤릭체*로 표기 (볼드체 ❌)

### Python

```python
x = 25
y = 3
print(x + y)  # 28
```

### NumPy

```python
import numpy as np
x = np.array(25)
print(x.shape)  # () ← 빈 튜플 = 0차원
```

### PyTorch

```python
import torch
x_pt = torch.tensor(25)
print(x_pt.shape)  # torch.Size([])
```

### TensorFlow

```python
import tensorflow as tf
x_tf = tf.Variable(25, dtype=tf.int16)
print(x_tf.shape)  # TensorShape([])
```

---

## 🔢 데이터 타입

```python
# 정수 (기본)
x = np.array(25)          # int64

# 실수
x = np.array(25.0)        # float64

# 명시적 지정
x = np.array(25, dtype=np.float16)
```

---

## 💡 핵심 포인트

- **Rank** = `ndim` = 대괄호 중첩 깊이
- **Shape** = 각 차원의 크기
- 스칼라는 shape이 빈 튜플 `()`
