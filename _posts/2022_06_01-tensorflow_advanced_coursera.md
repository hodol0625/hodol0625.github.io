---
title: "Tensorflow Coursera"
comments: true
# other options

categories:
    - tensorflow
  
published: false
---

### Gradient Tape

#### Gradient computation in tensorflow

예를 들어 $w^2$ 라는 loss 가 있으면 gradient는 $\frac{d}{dw}w^2=2w$ 이다.
이걸 tensorflow 에서 계산하려면 `GradientTape` 를 활용한다.

`GradientTape`은 자동으로 미분을 해주는 기능인데, context 안에서 실행된 모든 연산을 tape에 기록한다.

```python
w = tf.Varaible([[1.0]])
with tf.GradientTape() as tape:
    loss = w*w
```

- chain rule
  - 아래 코드를 보면,
    ```python
    x = tf.ones((2,2))
    with tf.GradientTape() as t:
        t.watch(x)

        y = tf.reduce_sum(x)
        z = tf.square(y)

    dz_dx = t.gradient(z,x)
    ```
    $\frac{\partial z}{\partial x}=\frac{\partial z}{\partial y} \frac{\partial y}{\partial x}$

    ![image](../assets/images/gradienttape.png)

- 실제 활용 방식

```python
def train_step(images, labels):
    with tf.GradientTape() as tape:
        # step1. 예측 결과
        logits = model(images, training=True)
        
        # step2. loss 계산
        loss_value = loss_object(labels, logits)
    
    loss_history.append(loss_value.numpy().mean())
    grads = tape.gradient(loss_value, model.trainable_variagles)
    optimizer.apply_gradients(zip(grads, model.trainable_variables))
```


###