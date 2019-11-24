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























참조
<https://umbum.tistory.com/223>
<https://untitledtblog.tistory.com/150>
