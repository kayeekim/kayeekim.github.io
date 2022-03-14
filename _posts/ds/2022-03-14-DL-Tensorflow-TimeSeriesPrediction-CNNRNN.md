---
layout: post
title:  "[DL] Tensorflow Tutorial 따라하기 - Time Series Prediction"
excerpt: "Tensorflow Tutorial 따라하기 - Time Series Prediction - CNN, RNN, etc."

categories:
  - ds
tags:
  - [DL]

toc: true
toc_sticky: true
 
date: 2022-03-14
last_modified_at: 2022-03-14

---

## Tensorflow Tutorial 따라하기 - Time Series Prediction

이 튜토리얼에서는 TensorFlow를 사용한 시계열 예측을 소개합니다. Convolutional/Recurrent Neural Network(CNN 및 RNN)를 포함하여 몇 가지 다른 스타일의 모델을 빌드합니다.

이 내용은 각각 하위 항목이 있는 두 부분으로 나누어 생각합니다.

단일 타임스텝 예측:
단일 특성
모든 특성
다중 스텝 예측:
싱글샷: 모두 한 번에 예측합니다.
자가 회귀: 한 번에 하나의 예측을 수행하고 결과를 모델로 피드백합니다.

### Code Practice - Github LINK. 
아래 링크를 따라가면 Practice 중인 ipynb 파일을 확인할 수 있습니다.

https://github.com/kayeekim00/kayeekim00.github.io/blob/master/_code_practice/Tensorflow_Time_Series_Prediction_KH.ipynb

----
#### Reference LINK.
https://www.tensorflow.org/tutorials/structured_data/time_series?hl=ko
