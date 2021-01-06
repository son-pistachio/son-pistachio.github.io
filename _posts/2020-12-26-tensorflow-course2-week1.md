---
title: DeepLearning.AI TensorFlow Developer Course 2-1
date: 2020-12-26
categories: [Deep Learning]
tags: [Deep Learning, Tensorflow, Keras, Coursera]
---

Building.....

**[Coursera] DeepLearning.AI TensorFlow Developer ( Tensorflow In Practice ) 강의 정리**  
Course 1 : Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning  
**Course 2 : Convolutional Neural Networks in TensorFlow**  

- **Week 1 - Exploring a Larger Dataset**
- Week 2 - Augmentation: A technique to avoid overfitting
- Week 3 - Transfer Learning
- Week 4 - Multiclass Classifications

Course 3 : Natural Language Processing in TensorFlow  
Course 4 : Sequences, Time Series and Prediction

---  

### Training with the cats vs. dogs dataset

[dogs or cats kaggle dataset](https://www.kaggle.com/c/dogs-vs-cats)

이미지 생성
```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

train_datagen = ImageDataGenerator( rescale = 1.0/255. )

train_generator = train_datagen.flow_from_directory(
		train_dir,
        target_size=(150, 150),
        batch_size=20,
        class_mode='binary',
        )     
```
`ImageDataGenerator(rescale = 1.0/255.)`&nbsp;&nbsp; - &nbsp;이미지 정규화  
`train_dir`&nbsp;&nbsp; - &nbsp;train 이미지가 포함 된 하위 디렉토리  
`target_size=(150, 150)`&nbsp;&nbsp; - &nbsp;이미지 사이즈 조정  
`class_mode='binary'`&nbsp;&nbsp; - &nbsp;이진분류

```python
test_datagen = ImageDataGenerator( rescale = 1.0/255. )

validation_generator =  test_datagen.flow_from_directory(
		validation_dir,
		target_size = (150, 150),
		batch_size=20,
		class_mode  = 'binary',
        )
```

모델 생성  
week1 사람 / 말 분류 방법과 유사
```python
model = tf.keras.models.Sequential([
    tf.keras.layers.Conv2D(16, (3,3), activation='relu', input_shape=(150, 150, 3)),
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Conv2D(32, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2), 
    tf.keras.layers.Conv2D(64, (3,3), activation='relu'), 
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Flatten(), 
    tf.keras.layers.Dense(512, activation='relu'), 
    tf.keras.layers.Dense(1, activation='sigmoid')  
])
```
`MaxPooling2D()`, `Conv2D` 세 세트로 구성


Output :  
{% highlight ruby %}
Model: "sequential" 
_________________________________________________________________ 
Layer (type)                    Output Shape          Param # 
================================================================= 
conv2d (Conv2D)                 (None, 148, 148, 16)  448 
_________________________________________________________________ 
max_pooling2d (MaxPooling2D) 	(None, 74, 74, 16)    0 
_________________________________________________________________ 
conv2d_1 (Conv2D)               (None, 72, 72, 32)    4640 
_________________________________________________________________ 
max_pooling2d_1 (MaxPooling2 	(None, 36, 36, 32)    0 
_________________________________________________________________ 
conv2d_2 (Conv2D)               (None, 34, 34, 64)    18496 
_________________________________________________________________ 
max_pooling2d_2 (MaxPooling2 	(None, 17, 17, 64)    0 
_________________________________________________________________ 
flatten (Flatten)               (None, 18496)         0 
_________________________________________________________________ 
dense (Dense)                   (None, 512)           9470464 
_________________________________________________________________ 
dense_1 (Dense)                 (None, 1)             513 
================================================================= 
Total params: 9,494,561 Trainable params: 9,494,561 Non-trainable params: 0 
_________________________________________________________________
{% endhighlight %}



```python
from tensorflow.keras.optimizers import RMSprop

model.compile(loss='binary_crossentropy',
              optimizer=RMSprop(lr=0.001), # learning rate 조정
              metrics = ['accuracy'])
```

```python
history = model.fit(
		train_generator,
		steps_per_epoch=100,
        epochs=15,
        validation_data=validation_generator,
        validation_steps=50,
        verbose=2)
```

---



```python
base_dir = '/tmp/cats_and_dogs_filtered'

train_dir = os.path.join(base_dir, 'train')
validation_dir = os.path.join(base_dir, 'validation')

# Directory with our training cat/dog pictures
train_cats_dir = os.path.join(train_dir, 'cats')
train_dogs_dir = os.path.join(train_dir, 'dogs')

# Directory with our validation cat/dog pictures
validation_cats_dir = os.path.join(validation_dir, 'cats')
validation_dogs_dir = os.path.join(validation_dir, 'dogs')
```


```python
train_cat_fnames = os.listdir( train_cats_dir )
train_dog_fnames = os.listdir( train_dogs_dir )

print(train_cat_fnames[:10])
print(train_dog_fnames[:10])

%matplotlib inline

import matplotlib.image as mpimg
import matplotlib.pyplot as plt

# Parameters for our graph; we'll output images in a 4x4 configuration
nrows = 4
ncols = 4

pic_index = 0 # Index for iterating over images
```

```python
# Set up matplotlib fig, and size it to fit 4x4 pics
fig = plt.gcf()
fig.set_size_inches(ncols*4, nrows*4)

pic_index+=8

next_cat_pix = [os.path.join(train_cats_dir, fname) 
                for fname in train_cat_fnames[ pic_index-8:pic_index] 
               ]

next_dog_pix = [os.path.join(train_dogs_dir, fname) 
                for fname in train_dog_fnames[ pic_index-8:pic_index]
               ]

for i, img_path in enumerate(next_cat_pix+next_dog_pix):
  # Set up subplot; subplot indices start at 1
  sp = plt.subplot(nrows, ncols, i + 1)
  sp.axis('Off') # Don't show axes (or gridlines)

  img = mpimg.imread(img_path)
  plt.imshow(img)

plt.show()

```

```python
model = tf.keras.models.Sequential([
    # Note the input shape is the desired size of the image 150x150 with 3 bytes color
    tf.keras.layers.Conv2D(16, (3,3), activation='relu', input_shape=(150, 150, 3)),
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Conv2D(32, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2), 
    tf.keras.layers.Conv2D(64, (3,3), activation='relu'), 
    tf.keras.layers.MaxPooling2D(2,2),
    # Flatten the results to feed into a DNN
    tf.keras.layers.Flatten(), 
    # 512 neuron hidden layer
    tf.keras.layers.Dense(512, activation='relu'), 
    # Only 1 output neuron. It will contain a value from 0-1 where 0 for 1 class ('cats') and 1 for the other ('dogs')
    tf.keras.layers.Dense(1, activation='sigmoid')  
])
```


```python
from tensorflow.keras.optimizers import RMSprop

model.compile(optimizer=RMSprop(lr=0.001),
              loss='binary_crossentropy',
              metrics = ['accuracy'])
```

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# All images will be rescaled by 1./255.
train_datagen = ImageDataGenerator( rescale = 1.0/255. )
test_datagen  = ImageDataGenerator( rescale = 1.0/255. )

# --------------------
# Flow training images in batches of 20 using train_datagen generator
# --------------------
train_generator = train_datagen.flow_from_directory(train_dir,
                                                    batch_size=20,
                                                    class_mode='binary',
                                                    target_size=(150, 150))     
# --------------------
# Flow validation images in batches of 20 using test_datagen generator
# --------------------
validation_generator =  test_datagen.flow_from_directory(validation_dir,
                                                         batch_size=20,
                                                         class_mode  = 'binary',
                                                         target_size = (150, 150))

```

```python
history = model.fit(train_generator,
                              validation_data=validation_generator,
                              steps_per_epoch=100,
                              epochs=15,
                              validation_steps=50,
                              verbose=2)
```

```python
import numpy as np

from google.colab import files
from keras.preprocessing import image

uploaded=files.upload()

for fn in uploaded.keys():
 
  # predicting images
  path='/content/' + fn
  img=image.load_img(path, target_size=(150, 150))
  
  x=image.img_to_array(img)
  x=np.expand_dims(x, axis=0)
  images = np.vstack([x])
  
  classes = model.predict(images, batch_size=10)
  
  print(classes[0])
  
  if classes[0]>0:
    print(fn + " is a dog")
    
  else:
    print(fn + " is a cat")
```


```python
import numpy as np
import random
from   tensorflow.keras.preprocessing.image import img_to_array, load_img

# Let's define a new Model that will take an image as input, and will output
# intermediate representations for all layers in the previous model after
# the first.
successive_outputs = [layer.output for layer in model.layers[1:]]

#visualization_model = Model(img_input, successive_outputs)
visualization_model = tf.keras.models.Model(inputs = model.input, outputs = successive_outputs)

# Let's prepare a random input image of a cat or dog from the training set.
cat_img_files = [os.path.join(train_cats_dir, f) for f in train_cat_fnames]
dog_img_files = [os.path.join(train_dogs_dir, f) for f in train_dog_fnames]

img_path = random.choice(cat_img_files + dog_img_files)
img = load_img(img_path, target_size=(150, 150))  # this is a PIL image

x   = img_to_array(img)                           # Numpy array with shape (150, 150, 3)
x   = x.reshape((1,) + x.shape)                   # Numpy array with shape (1, 150, 150, 3)

# Rescale by 1/255
x /= 255.0

# Let's run our image through our network, thus obtaining all
# intermediate representations for this image.
successive_feature_maps = visualization_model.predict(x)

# These are the names of the layers, so can have them as part of our plot
layer_names = [layer.name for layer in model.layers]

# -----------------------------------------------------------------------
# Now let's display our representations
# -----------------------------------------------------------------------
for layer_name, feature_map in zip(layer_names, successive_feature_maps):
  
  if len(feature_map.shape) == 4:
    
    #-------------------------------------------
    # Just do this for the conv / maxpool layers, not the fully-connected layers
    #-------------------------------------------
    n_features = feature_map.shape[-1]  # number of features in the feature map
    size       = feature_map.shape[ 1]  # feature map shape (1, size, size, n_features)
    
    # We will tile our images in this matrix
    display_grid = np.zeros((size, size * n_features))
    
    #-------------------------------------------------
    # Postprocess the feature to be visually palatable
    #-------------------------------------------------
    for i in range(n_features):
      x  = feature_map[0, :, :, i]
      x -= x.mean()
      x /= x.std ()
      x *=  64
      x += 128
      x  = np.clip(x, 0, 255).astype('uint8')
      display_grid[:, i * size : (i + 1) * size] = x # Tile each filter into a horizontal grid

    #-----------------
    # Display the grid
    #-----------------

    scale = 20. / n_features
    plt.figure( figsize=(scale * n_features, scale) )
    plt.title ( layer_name )
    plt.grid  ( False )
    plt.imshow( display_grid, aspect='auto', cmap='viridis' ) 
```

<br/>
> https://www.coursera.org/professional-certificates/tensorflow-in-practice
