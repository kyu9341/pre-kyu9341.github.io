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







## 실습

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














참조
<https://umbum.tistory.com/223>
<https://untitledtblog.tistory.com/150>
