---
title: DeepLearning.AI TensorFlow Developer Course 1-4
date: 2020-12-23
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow, Keras, Coursera]
---

**[Coursera] DeepLearning.AI TensorFlow Developer ( Tensorflow In Practice ) 강의 정리**  
**Course 1 : Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning**
- Week 1 - A New Programming Paradigm
- Week 2 - Introduction to Computer Vision
- Week 3 - Enhancing Vision with Convolutional Neural Networks
- **Week 4 - Using Real-world Images**

Course 2 : Convolutional Neural Networks in TensorFlow  
Course 3 : Natural Language Processing in TensorFlow  
Course 4 : Sequences, Time Series and Prediction

---  

**Week 4 - Using Real-world Images**

### Understanding ImageGenerator
ImageGenerator 모듈
```python
from tensorflow.keras.preprocessing.image
import ImageDataGenerator
```

Train 폴더
```python
train_datagen = ImageDataGenerator(rescale=1/255)

train_generator = train_datagen.flow_from_directory(
    train_dir,  
    target_size=(150, 150),  
    batch_size=20,
    class_mode='binary')  
```
상위 디렉토리 위치를 설정해야 함  
`train_dir`&nbsp;&nbsp; - &nbsp;train 이미지가 포함 된 하위 디렉토리  
`target_size=(150, 150)`&nbsp;&nbsp; - &nbsp;이미지 사이즈 조정  
`class_mode='binary'`&nbsp;&nbsp; - &nbsp;이진분류


validation 폴더
```python
test_datagen = ImageDataGenerator(rescale=1/255)

validation_generator = test_datagen.flow_from_directory( 
    validation_dir,
    target_size=(150, 150),  # 이미지 사이즈 조정
    batch_size=10,
    class_mode='binary')  # 이진분류
```
`validation_dir`&nbsp;&nbsp; - &nbsp;test 이미지가 포함 된 하위 디렉토리


### Defining a ConvNet to use complex images
사람 / 말 분류에 사용한 신경망
```python
model = tf.keras.models.Sequential([
   tf.keras.layers.Conv2D(16, (3,3), activation='relu', input_shape=(300, 300, 3)),
   tf.keras.layers.MaxPooling2D(2,2),
   tf.keras.layers.Conv2D(32, (3,3), activation='relu'),
   tf.keras.layers.MaxPooling2D(2,2),
   tf.keras.layers.Conv2D(32, (3,3), activation='relu'),
   tf.keras.layers.MaxPooling2D(2,2),
   tf.keras.layers.Flatten(),
   tf.keras.layers.Dense(512, activation='relu'),
   tf.keras.layers.Dense(1, activation='sigmoid')
])
```
세 개의 convolution, pooling layers 사용  
세 개의 convolution과 pooling layers는 이미지의 높은 복잡성과 크기를 반영  
`input_shape=(300, 300, 3)`&nbsp;&nbsp; - &nbsp;300 x 300 사이즈와 컬러이미지(빨간색, 초록색, 파란색)  
`tf.keras.layers.Dense(1, activation='sigmoid')`&nbsp;&nbsp; - &nbsp;sigmoid는 0 or 1이므로, 1로 출력  

### Training the ConvNet with fit_generator
```python
from tensorflow.keras.optimizers import RMSprop
model.compile(loss='binary_crossentropy',
              optimizer=RMSprop(lr=0.001),
              metrics=['acc'])
```
`loss='binary_crossentropy'`&nbsp;&nbsp; - &nbsp;사람/말 이진분류로 binary_crossentropy 선택  
`optimizer=RMSprop(lr=0.001)`&nbsp;&nbsp; - &nbsp;Learning Rate 조절


이미지 생성은 `model.fit()` 아닌 `model.fit_generator()` 사용 
```python
history = model.fit_generator(
      train_generator,		
      steps_per_epoch=8,	
      epochs
      validation_data=validation_generator, 
      validation_steps=8,
      verbose=2)			
```
`train_generator`&nbsp;&nbsp; - &nbsp;위의 train_generator 호출  
`steps_per_epoch=8`&nbsp;&nbsp;	- &nbsp;train image 1024개. 1024/8로 한 번에 128개 처리  
`validation_data=validation_generator`&nbsp;&nbsp; -&nbsp; validation 호출  
`validation_steps=8`&nbsp;&nbsp; - &nbsp;test image 256개. 256/8로 한 번에 32개 처리  
`verbose=2`&nbsp;&nbsp; -&nbsp; 진행사항 시각화 정도

전체코드
```python
# GRADED FUNCTION: train_happy_sad_model
def train_happy_sad_model():
    # Please write your code only where you are indicated.
    # please do not remove # model fitting inline comments.

    DESIRED_ACCURACY = 0.999

    class myCallback(tf.keras.callbacks.Callback):
         def on_epoch_end(self, epoch, logs={}):
                if(logs.get('acc')>DESIRED_ACCURACY):
                    print('\nacc is high')
                    self.model.stop_training = True

    callbacks = myCallback()
    
    # This Code Block should Define and Compile the Model. Please assume the images are 150 X 150 in your implementation.
    model = tf.keras.models.Sequential([
        tf.keras.layers.Conv2D(16, (3,3), activation='relu', input_shape=(150, 150, 3)),
        tf.keras.layers.MaxPooling2D(2,2),
        tf.keras.layers.Conv2D(32, (3,3), activation='relu'),
        tf.keras.layers.MaxPooling2D(2,2),
        tf.keras.layers.Conv2D(32, (3,3), activation='relu'),
        tf.keras.layers.MaxPooling2D(2,2),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(512, activation='relu'),
        tf.keras.layers.Dense(1, activation='sigmoid')
    ])

    from tensorflow.keras.optimizers import RMSprop

    model.compile(loss='binary_crossentropy',
              optimizer=RMSprop(lr=0.001),
              metrics=['acc'])
        
    model.summary()
    # This code block should create an instance of an ImageDataGenerator called train_datagen 
    # And a train_generator by calling train_datagen.flow_from_directory

    from tensorflow.keras.preprocessing.image import ImageDataGenerator

    train_datagen = ImageDataGenerator(rescale=1/255)

    # Please use a target_size of 150 X 150.
    train_generator = train_datagen.flow_from_directory(
        "/tmp/h-or-s",
        target_size=(150, 150),
        batch_size=20,
        class_mode='binary'
    )
    # Expected output: 'Found 80 images belonging to 2 classes'

    # This code block should call model.fit_generator and train for
    # a number of epochs.
    # model fitting
    history = model.fit_generator(
        train_generator,
        steps_per_epoch=8,
        epochs=15,
        verbose=1,
        callbacks=[callbacks]    
    )
    # model fitting
    return history.history['acc'][-1]
```

<br/>
> https://www.coursera.org/professional-certificates/tensorflow-in-practice
