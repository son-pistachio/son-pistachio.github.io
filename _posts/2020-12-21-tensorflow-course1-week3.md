---
title: DeepLearning.AI TensorFlow Developer Course 1-3
date: 2020-12-21
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow, Keras, Coursera]
---

**[Coursera] DeepLearning.AI TensorFlow Developer ( Tensorflow In Practice ) 강의 정리**  
**Course 1 : Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning**
- Week 1 - A New Programming Paradigm
- Week 2 - Introduction to Computer Vision
- **Week 3 - Enhancing Vision with Convolutional Neural Networks**
- Week 4 - Using Real-world Images

Course 2 : Convolutional Neural Networks in TensorFlow  
Course 3 : Natural Language Processing in TensorFlow  
Course 4 : Sequences, Time Series and Prediction

---  

**Week 3 - Enhancing Vision with Convolutional Neural Networks**

fashion mnist 데이터로 이어서 강의 진행  
앞서 사용한 기존 모델 코드
```python
import numpy as np
import tensorflow as tf

mnist = tf.keras.datasets.fashion_mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()

x_train, x_test = x_train / 255.0, x_test / 255.0

model = tf.keras.models.Sequential([
tf.keras.layers.Flatten(input_shape=(28, 28)),
tf.keras.layers.Dense(512, activation=tf.nn.relu),
tf.keras.layers.Dense(10, activation=tf.nn.softmax)
])
model.compile(optimizer=tf.optimizers.Adam(),
loss='sparse_categorical_crossentropy',
metrics=['accuracy'])
model.fit(x_train, y_train, epochs=10)
```

### Implementing convolutional layers
기존 모델
```python
model = tf.keras.models.Sequential([
tf.keras.layers.Flatten(),
tf.keras.layers.Dense(128, activation=tf.nn.relu),
tf.keras.layers.Dense(10, activation=tf.nn.softmax)
])
```

convolutional layer 사용하는 모델  
마지막 세 줄은 기존 모델과 동일  
`tf.keras.layers.Flatten()`  
`tf.keras.layers.Dense(128, activation='relu')`  
`tf.keras.layers.Dense(10, activation='softmax')`

```python
model = tf.keras.models.Sequential([
tf.keras.layers.Conv2D(64, (3, 3), activation='relu', input_shape=(28,28,1)),
tf.keras.layers.MaxPooling2D(2, 2),
tf.keras.layers.Conv2D(64, (3, 3), activation='relu'),
tf.keras.layers.MaxPooling2D(2, 2),
tf.keras.layers.Flatten(),
tf.keras.layers.Dense(128, activation='relu'),
tf.keras.layers.Dense(10, activation='softmax')
])
```
`tf.keras.layers.Conv2D(64, (3, 3), activation='relu', input_shape=(28,28,1))`  
첫 번째 convolution 지정  
karas에 64개 필터 생성을 요청 - 필터는 3 x 3이고, actiovation은 relu, 입력형태는 28 x 28이다.  
추가한 1은 색 깊이를 의미한다.

---
### Implementing pooling layers
```python
model = tf.keras.models.Sequential([
tf.keras.layers.Conv2D(64, (3, 3), activation='relu', input_shape=(28,28,1)),
tf.keras.layers.MaxPooling2D(2, 2),
tf.keras.layers.Conv2D(64, (3, 3), activation='relu'),
tf.keras.layers.MaxPooling2D(2, 2),
tf.keras.layers.Flatten(),
tf.keras.layers.Dense(128, activation='relu'),
tf.keras.layers.Dense(10, activation='softmax')
])

model.summary()
```
`tf.keras.layers.MaxPooling2D(2, 2)` 2 x 2 pooling layer  
4 픽셀마다 가장 큰 수를 사용

`model.summary()` : 모델의 레이어 검사하고 합성곱을 통해 이동 경로 확인  
Output :  
{% highlight ruby %}
Model: "sequential_3"
_________________________________________________________________ 
Layer (type)                    Output Shape          Param # 
================================================================= 
conv2d_6 (Conv2D)               (None, 26, 26, 64)    640 
_________________________________________________________________ 
max_pooling2d_4 (MaxPooling2 	(None, 13, 13, 64)    0 
_________________________________________________________________ 
conv2d_7 (Conv2D)               (None, 11, 11, 64)    36928 
_________________________________________________________________ 
max_pooling2d_5 (MaxPooling2 	(None, 5, 5, 64)      0 
_________________________________________________________________ 
flatten_3 (Flatten)             (None, 1600)          0 
_________________________________________________________________ 
dense_6 (Dense)                 (None, 128)           204928 
_________________________________________________________________ 
dense_7 (Dense)                 (None, 10)            1290 
================================================================= 
Total params: 243,786 
Trainable params: 243,786 
Non-trainable params: 0 
_________________________________________________________________
{% endhighlight %}

처음 26, 26은 필터가 3 x 3으로 모서리 부분은 지날 수 없으므로, x와 y의 픽셀을 하나씩 제거  
maxpooling에 의해 이미지가 축소  
다음 convolutional layer도 x와 y 한 픽셀씩 제거  
마지막 maxpooling을 거치면, 5x5의 64개 이미지 형성

---
### Improving the Fashion classifier with convolutions

`filter` - 이미지 전달하여 정보의 양을 줄이고, neural network가 한 종류의 이미지를 다른 종류와 구별할 수 있는 특징 추출  
`pooling` - 정보를 더 쉽게 관리하기 위해 압축하는 방법

<br/>
> https://www.coursera.org/professional-certificates/tensorflow-in-practice
