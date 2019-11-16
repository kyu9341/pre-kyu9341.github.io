---
layout: post
title: "Machine Learning"
subtitle: "Machine Learning"
date: 2019-11-10 12:34:22
author: kwon
categories: MachineLearning
---
이번에 한이음에서 주최하는 ICT멘토링 MS AI머신러닝(기초)교육에 참여하게 되었다. 토일 10:00 ~ 18:00까지 2일에 걸쳐서 진행되는데 첫째 날에는 인공지능 개론/사례, 머신러닝 이론, Classification모델에 대해서 배우고, 둘째 날에는 ~~


## 인공지능이란?
기계를 인간과 비슷하게 동작하게 하는 기술이다. 인간이 사고하는 과정처럼 인식(보고, 듣고) -> 이해(학습, 분석) -> 반응(결과)의 순으로 진행이된다.

- 인공지능 : 기계 혹은 컴퓨터가 인간의 지능을 모방해 인간과 비슷하게 동작하도록 만들어진 기술

- 머신러닝 : 인공지능의 한 분야. 컴퓨터가 데이터를 이용해 학습하는 알고리즘 기술
ex) 인공신경망, 결정 트리, 벡터 머신 등

- 딥러닝 : 인공신경망을 사용하는 머신러닝 모델링 방법 중 하나
다층 인공신경망 구조를 사용하여 빅 데이터 학습

ex) 사물인식, 감정분석, 필기체 인식, 음성인식 등

## 머신러닝의 종류
* Supervised Learning(지도학습, 감독학습)
  - 문제와 함께 정답을 제공(Feature & Label)
  - 예측(Regression), 추정(Forecast), 분류(Classification) 등의 문제 해결 시 주로 사용
  - 만들기 쉽고, 성능도 좋음

* Unsupervised Learning (비지도학습)
  -	문제만 제공 (Feature)
  -	패턴/구조 발견 (Anomaly Detection) : 평소와 다른 점 발견 (이상감지) ex) 지진, 의료 등 이상 징후 감지
  -	그룹화 (Clustering) : 여러 데이터를 input, 알아서 비슷한 것들을 모아줌
* Reinforcement Learning (강화 학습)
  -	정답이 아닌 보상 제공
  -	인과관계가 중요
  -	게임(알파고), 로봇
  -	사람의 움직임을 따라 움직이는 아바타











첫째 날의 실습 환경은 MS의 클라우드 서비스인 Azure를 사용한 Azure ML Studio를 사용했다.
