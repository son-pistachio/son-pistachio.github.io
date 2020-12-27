---
title: DeepLearning.AI TensorFlow Developer Course 1-2
date: 2020-12-20
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow, Keras, Coursera]
---

**[Coursera] DeepLearning.AI TensorFlow Developer ( Tensorflow In Practice ) 강의 정리**  
**Course 1 : Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning**
- Week 1 - A New Programming Paradigm
- **Week 2 - Introduction to Computer Vision**
- Week 3 - Enhancing Vision with Convolutional Neural Networks
- Week 4 - Using Real-world Images

Course 2 : Convolutional Neural Networks in TensorFlow  
Course 3 : Natural Language Processing in TensorFlow  
Course 4 : Sequences, Time Series and Prediction

---  

**Week 2 - Introduction to Computer Vision**

### An Introduction to computer vision

Fashion MNIST dataset
- 70k Images  
70,000개 이미지
- 10 Categories  
10가지 의류
- Images are 28x28  
28 x 28 픽셀 : 각 픽셀은 0 - 255 표현
- Can train a neural net!

---
### Writing code to load training data
```python
mnist = keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()
```
총 70,000개 이미지  
train - 60,000개  
test - 10,000개  

| 레이블 | 클래스 |
| :--: | :--: |
| 0 | T-shirt/top |
| 1 | Trouser |
| 2 | Pullover |
| 3 | Dress |
| 4 | Coat |
| 5 | Sandal |
| 6 | Shirt |
| 7 | Sneaker |
| 8 | Bag |
| 9 | Ankle boot |

> https://www.tensorflow.org/tutorials/keras/classification

---
### Coding a Computer Vision Neural Network
CNN 모델링 준비
```python
model = tf.keras.models.Sequential([
		tf.keras.layers.Flatten(input_shape=(28, 28)),
		tf.keras.layers.Dense(512, activation=tf.nn.relu),
		tf.keras.layers.Dense(10, activation=tf.nn.softmax)
])
```

첫번째 레이어  
`tf.keras.layers.Flatten(input_shape=(28, 28))`  
이미지가 28 x 28이므로, input shape는 28x28 flatten layer이다.

마지막 레이어  
10개 뉴런이 있는 이유는 데이터 셋에 10가지 종류 의류가 있기 때문이다.

---
### Walk through a Notebook for computer vision
이미지는 0 ~ 255 사이 값으로 표현된다.  
모든 값을 0 ~ 1 변경하기 위해 255로 나눈다.
```python
(x_train, y_train),(x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0
```

---
### Using Callbacks to control training
Callback은 사용자가 지정한 시점에서 학습을 취소 할 수 있다.  

```python
class myCallback(tf.keras.callbacks.Callback):
	def on_epoch_end(self, epoch, logs={}):
		if(logs.get('loss')<0.4):
			print("\nLoss is low so cancelling training!")
			self.model.stop_training = True
```
예를 들어, 학습 중 Loss가 0.4미만이면 학습을 취소한다.

---
### Walk through a notebook with Callbacks
전체 코드
```python
import tensorflow as tf

class myCallback(tf.keras.callbacks.Callback):
	def on_epoch_end(self, epoch, logs={}):
		if(logs.get('loss')<0.4):
			print("\nLoss is low so cancelling training!")
			self.model.stop_training = True

mnist = keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = mnist.load_data()

train_images, test_images = train_images / 255.0, test_images / 255.0

callbacks = myCallback()
model = tf.keras.models.Sequential([
		tf.keras.layers.Flatten(input_shape=(28, 28)),
		tf.keras.layers.Dense(512, activation=tf.nn.relu),
		tf.keras.layers.Dense(10, activation=tf.nn.softmax)
		])
model.compile(optimizer=tf.optimizers.Adam(),
loss='sparse_categorical_crossentropy',
metrics=['accuracy'])
model.fit(x_train, y_train, epochs=10, callbacks=[callbacks])
```

**callbacks 사용보다는 끝까지 기다리는 것을 추천!**
