---
layout: post
title: "ICT멘토링 딥러닝 교육 2일차"
subtitle: "Machine Learning"
date: 2019-11-24 10:01:25
author: kwon
categories: MachineLearning
---
이번에는 둘째 날 배운 내용을 리뷰해보겠다. 둘째 날에는 이미지 분류 이론, Azure Cloud GPU활용, Cifar10 이미지 분류에 대해서 배웠다.

### Convolutional Neural Network(CNN)
CNN(합성곱 신경망)은 필터링 기법을 인공신경망에 적용함으로써 이미지를 더욱 효과적으로 처리하기 위해 고안되었다. 이미지를 Dense(fully connected) Layer로 처리하려 한다면 feature가 너무 많아져 불필요한 weight가 많아지고 효율이 떨어지게 된다. 또한 Dense Layer는 1차원 데이터만 input으로 받을 수 있기 때문에 3차원 데이터를 평탄화하여 입력해야 한다. 여기서 3차원 데이터의 공간적 정보가 소실되는 문제가 발생한다. 반면 Convolutional Layer에서는 형상을 유지한다. 입/출력 모두 3차원 데이터로 처리하기 때문에 공간적 정보를 유지할 수 있다.

#### Convolutional Layer
Convolution은 합성곱이라는 뜻이다.







## 실습1

첫번째는 간단하게 이미지만 불러와서 확인하는 과정이다.

#### 2019.11.24. 딥-러닝 과정 CNN

### 준비 실습. Image 살펴보기


```python
# 1. package 가져오기

from PIL import Image

```


```python
# 2. 이미지 가져오기

image_gray = Image.open('01image.png')
image_color = Image.open('02image.png')
image_jet = Image.open('jet.png')


print(image_gray.size)
print(image_gray)
# print(image_jet)


print(image_color.size) # size만 뽑으면 가로 세로만 나옴
print(image_gray.size)
# print(image_jet.size)
```

    (28, 28)
    <PIL.PngImagePlugin.PngImageFile image mode=L size=28x28 at 0x1405C55AE08>
    (32, 32)
    (28, 28)



```python
# 3. 이미지 array로 변환하기

import numpy as np

image_array = np.array(image_gray)
image_array_color = np.array(image_color)
# image_array_jet = np.array(image_jet)


print(image_array.shape) # 학습 시킬때 항상 numpy data로 변환하며 shape를 확인해야함
print(image_array_color.shape) # numpy array로 변환하여 출력하면 (32, 32, 3)처럼 뎁스도 출력 가능
# 3차원 데이터여야만 CNN에 넣어서 학습이 가능 따라서 gray scale은 reshape해줘야함
# print(image_array_jet.shape)
reshpaed_1d = image_array.reshape(28*28) # gray image ->reshape
reshaped_3d = image_array.reshape(28, 28, 1) # gray image -> 3d로 변경하여 cnn이 가능하도록 함

print(reshpaed_1d.shape)
print(reshaped_3d.shape)
```

    (28, 28)
    (32, 32, 3)
    (784,)
    (28, 28, 1)



```python
# 4. 이미지 살펴보기

# print(image_array)
print(image_array_color)
# print(image_array_jet)
# print(reshaped_3d)

```

    [[[125 125 116]
      [110 101  91]
      [102  90  83]
      ...
      [202 207 214]
      [200 205 212]
      [202 208 214]]
      ...
      [143 117  82]
      [143 116  84]
      [144 116  86]]]



```python
# 5. 이미지 시각화
import matplotlib.pyplot as plt

plt.imshow(image_array, cmap=plt.cm.gray) # gray scale를 정상적으로 띄우려면 color map 을 지정해야함
plt.imshow(image_array_color)
# plt.imshow(image_array_jet)
```

    <matplotlib.image.AxesImage at 0x1405c59de48>


<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/output_6_1.png" style="width: 50%
    ; height: 300px;">
</div>


## 실습2
두번째 실습은 CNN모델을 적용하기 전에




#### 2019.11.24. 딥-러닝 과정 CNN

### 첫번째 실습. Keras 모델 생성/학습 - MNIST : MLP
[Keras Dataset](https://keras.io/ko/datasets/#mnist)


```python
# 1. 데이터 불러오기

from keras.datasets import mnist  # 많이 쓰이는 데이터 케라스에서 제공

(X_train, y_train), (X_test, y_test) = mnist.load_data()

print(X_train.shape)
print(y_train.shape)
print(X_test.shape)
print(y_test.shape)
```

    (60000, 28, 28)
    (60000,)
    (10000, 28, 28)
    (10000,)



```python
# 2. 이미지 데이터 확인하기 🖼
import matplotlib.pyplot as plt

image = X_train[1]

plt.imshow(image, cmap=plt.cm.gray)
```

    <matplotlib.image.AxesImage at 0x12a2b101c48>

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/output_3_1.png" style="width: 50%
    ; height: 300px;">
</div>


```python
print(X_train.shape) # 28*28 이미지 (2차원) 6만개 -> 1차원 6만개
```

    (60000, 28, 28)


```python
# 3-1. 이미지 데이터 전처리 : 2차원->1차원 🌟🌟🌟

X_train = X_train.reshape((60000, 28 * 28)) # -> 1차원 6만개로 변환
X_test = X_test.reshape((10000, 28 * 28))

print(X_train.shape)
print(X_test.shape)

print(X_train)
```

    (60000, 784)
    (10000, 784)
    [[0 0 0 ... 0 0 0]
     [0 0 0 ... 0 0 0]
     [0 0 0 ... 0 0 0]
     ...
     [0 0 0 ... 0 0 0]
     [0 0 0 ... 0 0 0]
     [0 0 0 ... 0 0 0]]


```python
# 3-2. 이미지 데이터 전처리 : Normalzation
X_train = X_train / 255.0
X_test = X_test / 255.0

print(X_train[9])
```



```python
# 4. Label 전처리 (one-hot encoding)
print(y_train[:10]) # 앞에서 10개, label : 숫자가 어떤 숫자인지
```

    [5 0 4 1 9 2 1 3 1 4]



```python
# 4. Label 전처리 (one-hot encoding)

from keras.utils import to_categorical
y_train = to_categorical(y_train)
y_test = to_categorical(y_test)

print(y_train[:10])
```

    [[0. 0. 0. 0. 0. 1. 0. 0. 0. 0.]
     [1. 0. 0. 0. 0. 0. 0. 0. 0. 0.]
     [0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]
     [0. 1. 0. 0. 0. 0. 0. 0. 0. 0.]
     [0. 0. 0. 0. 0. 0. 0. 0. 0. 1.]
     [0. 0. 1. 0. 0. 0. 0. 0. 0. 0.]
     [0. 1. 0. 0. 0. 0. 0. 0. 0. 0.]
     [0. 0. 0. 1. 0. 0. 0. 0. 0. 0.]
     [0. 1. 0. 0. 0. 0. 0. 0. 0. 0.]
     [0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]]



```python
# 4-1 Validation 셋 나누기

from sklearn.model_selection import train_test_split

X_train, X_val, y_train, y_val = train_test_split(X_train, y_train,
                                                    test_size=0.2,
                                                    random_state=9)

print(X_train.shape)
print(y_train.shape)

print(X_val.shape)
print(y_val.shape)


```

    (48000, 784)
    (48000, 10)
    (12000, 784)
    (12000, 10)



```python
# 5. 모델 생성 : MLP
from keras.models import Sequential
from keras.layers import Dense

model = Sequential()
model.add(Dense(512, input_dim=784, activation='relu')) # 다음 노드의 수 : 512(inputdata가 784이므로 적당히 크게)
model.add(Dense(10, activation='softmax')) # 출력 층 10개중 어떤건지 알기 위해 10으로 지정

print(model.summary())
```

    Model: "sequential_6"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    dense_9 (Dense)              (None, 512)               401920    
    _________________________________________________________________
    dense_10 (Dense)             (None, 10)                5130      
    =================================================================
    Total params: 407,050
    Trainable params: 407,050
    Non-trainable params: 0
    _________________________________________________________________
    None



```python
# 6. Compile - Optimizer, Loss function 설정
model.compile(loss='categorical_crossentropy',
              optimizer='sgd',
              metrics=['accuracy'])
```


```python
# 7. 모델 학습시키기

batch_size=128
epochs=10

history = model.fit(X_train, y_train,
         epochs=epochs,
         batch_size=batch_size,
         validation_data=(X_val, y_val), # validation_set 적용 (꼭 같이 해주는게 좋음)
         verbose=1)
```

    Train on 48000 samples, validate on 12000 samples
    Epoch 1/10
    48000/48000 [==============================] - 2s 37us/step - loss: 1.2137 - accuracy: 0.7386 - val_loss: 0.6995 - val_accuracy: 0.8526
    Epoch 2/10
    48000/48000 [==============================] - 2s 34us/step - loss: 0.5725 - accuracy: 0.8683 - val_loss: 0.4960 - val_accuracy: 0.8789
    Epoch 3/10
    48000/48000 [==============================] - 2s 42us/step - loss: 0.4501 - accuracy: 0.8855 - val_loss: 0.4243 - val_accuracy: 0.8904
    Epoch 4/10
    48000/48000 [==============================] - 2s 40us/step - loss: 0.3967 - accuracy: 0.8948 - val_loss: 0.3847 - val_accuracy: 0.8984
    Epoch 5/10
    48000/48000 [==============================] - 2s 40us/step - loss: 0.3648 - accuracy: 0.9019 - val_loss: 0.3599 - val_accuracy: 0.9035
    Epoch 6/10
    48000/48000 [==============================] - 2s 40us/step - loss: 0.3425 - accuracy: 0.9064 - val_loss: 0.3419 - val_accuracy: 0.9077
    Epoch 7/10
    48000/48000 [==============================] - 2s 38us/step - loss: 0.3257 - accuracy: 0.9103 - val_loss: 0.3276 - val_accuracy: 0.9109
    Epoch 8/10
    48000/48000 [==============================] - 2s 40us/step - loss: 0.3121 - accuracy: 0.9145 - val_loss: 0.3155 - val_accuracy: 0.9133
    Epoch 9/10
    48000/48000 [==============================] - 2s 41us/step - loss: 0.3006 - accuracy: 0.9174 - val_loss: 0.3064 - val_accuracy: 0.9160
    Epoch 10/10
    48000/48000 [==============================] - 2s 40us/step - loss: 0.2906 - accuracy: 0.9201 - val_loss: 0.2977 - val_accuracy: 0.9183



```python
# 8. 모델 평가하기
test_loss, test_acc = model.evaluate(X_test, y_test)

print(test_loss, test_acc)
```

    10000/10000 [==============================] - 0s 25us/step
    0.2771936253398657 0.9223999977111816



```python
# 9. 이미지를 랜덤으로 선택해 훈련된 모델로 예측 🖼

import numpy
for index in numpy.random.choice(len(y_test), 3, replace = False):
    predicted = model.predict(X_test[index:index + 1])[0]
    label = y_test[index]
    result_label = numpy.where(label == numpy.amax(label))
    result_predicted = numpy.where(predicted == numpy.amax(predicted))
    title = "Label value = %s  Predicted value = %s " % (result_label[0], result_predicted[0])

    fig = plt.figure(1, figsize = (3,3))
    ax1 = fig.add_axes((0,0,.8,.8))
    ax1.set_title(title)
    images = X_test
    plt.imshow(images[index].reshape(28, 28), cmap = 'Greys', interpolation = 'nearest')
    plt.show()
```

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/output_14_0.png" style="width: 50%
    ; height: 300px;">
</div>

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/output_14_1.png" style="width: 50%
    ; height: 300px;">
</div>

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/output_14_2.png" style="width: 50%
    ; height: 300px;">
</div>


```python
# 10. 학습 시각화하기
import matplotlib.pyplot as plt

plt.plot(history.history['val_accuracy'])
plt.plot(history.history['accuracy'])
plt.title('Accuracy')
plt.xlabel('epoch')
plt.ylabel('accuracy')
plt.legend(['train', 'test'], loc='upper left')
plt.show()



# plt.plot(history.history['val_accuracy'])

```

<div style="width: 100%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/output_15_0.png" style="width: 50%
    ; height: 300px;">
</div>



## 실습 3







참조
<https://umbum.tistory.com/223>
<https://untitledtblog.tistory.com/150>
