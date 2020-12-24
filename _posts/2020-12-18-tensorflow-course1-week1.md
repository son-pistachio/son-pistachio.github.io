---
title: Tensorflow In Practice Course 1-1
date: 2020-12-18
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow, Keras, Coursera]
---

**[Coursera] Tensorflow In Practice 강의 정리**
**Course 1 : Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning**  
Course 2 : Convolutional Neural Networks in TensorFlow  
Course 3 : Natural Language Processing in TensorFlow  
Course 4 : Sequences, Time Series and Prediction


## Week 1 정리

### The ‘Hello World’ of neural networks
X = -1, 0, 1, 2, 3, 4  
Y = -3, -1, 1, 3, 5, 7  
⇨ Y = 2X - 1

필수 라이브러리, 모듈, 프레임워크 불러오기
```python
import tensorflow as tf
import numpy as np
from tensorflow import keras
```

파이썬과 keras라는 텐서플로우 API 사용
```python
model = tf.keras.Sequential([keras.layers.Dense(units=1, input_shape=[1])])
```
Keras 사용하면, 신경망을 쉽게 정의 할 수 있다.  
신경망은 기본적으로 패턴을 배울 수 있는 기능

Loss Function, optimizer 함수 역할
```python
model.compile(optimizer='sgd', loss='mean_squared_error')
```
Loss Function은 값을 측정한 후 Optimizer에 데이터를 제공하여 다음 예측 값을 알아낸다.  
Optimizer는 Loss Function 데이터를 사용하여 예측이 얼마나 잘 수행되었는지 또는 잘못 수행되었는지를 측정한다.  
`loss='mean_squared_error'`, `optimizer='sgd'`  
sgd(stochastic gradient descent)

```python
# next step
xs = np.array([-1.0, 0.0, 1.0, 2.0, 3.0, 4.0], dtype=float)
ys = np.array([-3.0, -1.0, 1.0, 3.0, 5.0, 7.0], dtype=float)
```
np.array는 Python 라이브러리의 numpy 사용

```python
model.fit(xs, ys. epochs=500)
```
fit 명령어로 학습 진행.
`epochs=500` 훈련 루프를 500번 통과

```python
print(model.predict([10.0]))
```
모델이 학습을 마치면, 예측 값을 출력  
숫자 10이 예측모델을 통과 할 때,  Y = 2X - 1으로 19를 반환 할 것이라고 생각한다.  
그러나 19에 매우 가깝지만 정확히 19가 아닌 값을 반환한다.

이유는  
- 아주 적은 데이터를 사용하여 훈련했다.  
  위 6개 점은 선형이지만 모든 X에 대해 관계가 Y = 2X - 1과 같을 것이라는 보장은 없다. 신경망에서 X = 10이면, Y가 19일 확률이 매우 높지만 정수는 아니다.
- 신경망이 답을 알아 내려고 할 때 확률로 처리한다.


<br/>
> https://www.coursera.org/professional-certificates/tensorflow-in-practice
