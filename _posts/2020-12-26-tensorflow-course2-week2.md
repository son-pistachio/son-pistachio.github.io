---
title: DeepLearning.AI TensorFlow Developer Course 2-2
date: 2020-12-28
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow, Keras, Coursera]
---

**[Coursera] DeepLearning.AI TensorFlow Developer ( Tensorflow In Practice ) 강의 정리**  
Course 1 : Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning  
**Course 2 : Convolutional Neural Networks in TensorFlow**  
- Week 1 - Exploring a Larger Dataset
- **Week 2 - Augmentation: A technique to avoid overfitting**
- Week 3 - Transfer Learning
- Week 4 - Multiclass Classifications

Course 3 : Natural Language Processing in TensorFlow  
Course 4 : Sequences, Time Series and Prediction

---  

## Image Augmentation

Image Augmentation : 데이터 과적합 방지 위한 매우 강력한 도구  
요약하면, 만약 고양이를 발견하기 위해 모델을 학습시키고 있고, 모델은 고양이가 누워있을 때 데이터가 없다면,  
그 것을 인식하지 못할 수 있다.  
이에, Augmentation은 회전, 줌 같은 변환을 사용하여 학습하는 동안 이미지를 즉시 수정한다.  
예를 들어, '서 있는' 고양이를 90도 회전시켜 누운 고양이의 이미지처럼 만들 수 있다.  
그리고 데이터 세트를 기존 데이터셋보다 확장할 수 있다.

[https://github.com/keras-team/keras-preprocessing](https://github.com/keras-team/keras-preprocessing)에서 Augmentation 관하여 자세히 확인할 수 있다.  
```python
train_datagen = ImageDataGenerator(
      rescale=1./255,
      rotation_range=40,
      width_shift_range=0.2,
      height_shift_range=0.2,
      shear_range=0.2,
      zoom_range=0.2,
      horizontal_flip=True,
      fill_mode='nearest')
```
`rescale=1./255` &nbsp;&nbsp; - &nbsp; 스케일 조정  
`rotation_range=40` &nbsp;&nbsp; - &nbsp; 0-180도 회전 범위  
`width_shift_range=0.2` &nbsp;&nbsp; - &nbsp; 프레임 안쪽으로 이미지 수평 20% 이동  
`height_shift_range=0.2` &nbsp;&nbsp; - &nbsp; 프레임 안쪽으로 이미지 수직 20% 이동  
`shear_range=0.2` &nbsp;&nbsp; - &nbsp; 전단  
`zoom_range=0.2` &nbsp;&nbsp; - &nbsp; 줌  
`horizontal_flip=True` &nbsp;&nbsp; - &nbsp; 오른쪽 왼쪽 전환  
`fill_mode='nearest'` &nbsp;&nbsp; - &nbsp; 위 작업으로 손실될 픽셀 채워 줌



## The impact of augmentation
전체코드
```python
!wget --no-check-certificate \
    https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip \
    -O /tmp/cats_and_dogs_filtered.zip
  
import os
import zipfile
import tensorflow as tf
from tensorflow.keras.optimizers import RMSprop
from tensorflow.keras.preprocessing.image import ImageDataGenerator

local_zip = '/tmp/cats_and_dogs_filtered.zip'
zip_ref = zipfile.ZipFile(local_zip, 'r')
zip_ref.extractall('/tmp')
zip_ref.close()

base_dir = '/tmp/cats_and_dogs_filtered'
train_dir = os.path.join(base_dir, 'train')
validation_dir = os.path.join(base_dir, 'validation')

# Directory with our training cat pictures
train_cats_dir = os.path.join(train_dir, 'cats')

# Directory with our training dog pictures
train_dogs_dir = os.path.join(train_dir, 'dogs')

# Directory with our validation cat pictures
validation_cats_dir = os.path.join(validation_dir, 'cats')

# Directory with our validation dog pictures
validation_dogs_dir = os.path.join(validation_dir, 'dogs')

model = tf.keras.models.Sequential([
    tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(150, 150, 3)),
    tf.keras.layers.MaxPooling2D(2, 2),
    tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(512, activation='relu'),
    tf.keras.layers.Dense(1, activation='sigmoid')
])

model.compile(loss='binary_crossentropy',
              optimizer=RMSprop(lr=1e-4),
              metrics=['accuracy'])

# All images will be rescaled by 1./255
train_datagen = ImageDataGenerator(rescale=1./255)
test_datagen = ImageDataGenerator(rescale=1./255)

# Flow training images in batches of 20 using train_datagen generator
train_generator = train_datagen.flow_from_directory(
        train_dir,  # This is the source directory for training images
        target_size=(150, 150),  # All images will be resized to 150x150
        batch_size=20,
        # Since we use binary_crossentropy loss, we need binary labels
        class_mode='binary')

# Flow validation images in batches of 20 using test_datagen generator
validation_generator = test_datagen.flow_from_directory(
        validation_dir,
        target_size=(150, 150),
        batch_size=20,
        class_mode='binary')

history = model.fit(
      train_generator,
      steps_per_epoch=100,  # 2000 images = batch_size * steps
      epochs=100,
      validation_data=validation_generator,
      validation_steps=50,  # 1000 images = batch_size * steps
      verbose=2)
```
