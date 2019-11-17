---
layout: post
title: "ICT멘토링 머신러닝 교육2"
subtitle: "Machine Learning"
date: 2019-11-17 21:09:22
author: kwon
categories: MachineLearning
---
이번에는 둘째 날에 배웠던 내용에 대해 리뷰해보도록 하겠다. 둘째 날은 Regression모델, 머신러닝 알고리즘, python라이브러리인 scikit-learn을 이용하여 실습을 진행하였다.

오전에는 어제 했던 Classification모델에 이어서 Azure ML Studio에서 Regression모델을 실습해보았다.
<div style="width: 70%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/ai3.png" style="width: 70%
    ; height: 300px;">
</div>
Regression모델 같은 경우는 위와 같이 x값에 따른 y의 값을 찾고 싶을 때 사용한다.

 데이터 셋으로는 예전에 Kaggle에서 올라왔던 월마트 판매량 예측을 위한 데이터 셋을 이용하였다. 이 데이터는 어제 사용했던 것들과는 달리 3개의 데이터 셋이 따로 존재해서 이 세개의 데이터를 합치는 과정이 필요하다.

<div style="width: 80%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/machine8.png" style="width: 80%
    ; height: 400px;">
</div>

 먼저 featrue.csv파일을 select columns in datset에서 필요하지 않은 컬럼을 제외시키고 train.csv파일을 가져와 join data로 합쳐주었다. 이 때, 지점과 날짜를 키값으로 하여 합쳤고 다시 stores.csv파일까지  Join Data로 지점을 키값으로 하여 합쳤다.

이후 Edit Metadata에서 숫자데이터이지만 string형으로 저장되어 있는 CPI, Unemployment를 float형으로 변환해주었다. 다음으로 예측하고 싶은 데이터인 Weekly Sales(주간판매량)을 레이블로 설정해주고 지점, 부서, 유형을 make categorical해주었다. (다른 데이터를 입력받지 않을 것이고, 상하관계가 없으므로)

이제 데이터 전처리가 끝났으므로 모델을 선택하여 학습을 시켜준다.

Linear Regression이라는 알고리즘을 사용하여 학습시켜보았다.

<div style="width: 80%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/regression1.png" style="width: 80%
    ; height: 400px;">
</div>
위의 이미지과 같이 Scored Labels는 예측 데이터이고 주간 판매량이 실제 데이터이다.

아래 이미지는 Boosted Decision Tree Regression알고리즘을 사용하여 한번 더 예측하고 Linear Regression와 비교해본 것이다. 왼쪽이 Linear Regression이고 오른쪽이 Boosted Decision Tree Regression이다.  

<div style="width: 90%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/regression2.png" style="width: 100%
    ; height: 300px;">
</div>
