---
layout: post
title:  "[DA/ML] 시계열 분석 Time Series"
excerpt: "Time Series Analytics (a.k.a ARIMA)"

categories:
  - Blog
tags:
  - [DA/ML]

toc: true
toc_sticky: true
 
date: 2022-02-07
last_modified_at: 2022-02-07
---

## Summary


## 시계열 분석 (Time Series)
-. 시간에 따른 연속형 변수의 예측 / Trend 파악. 

-. 시간의 특정 간격 (lag) 에서 data의 trend 파악.

-. 이전에 일어난 일들이 다음에 일어날 일들에 어떤 영향을 줄지를 찾는 것


### 시계열 패턴
1) trend 추세
2) seasonality 계절성
3) cycle 주기
4) noise 잡음; 시간에 따라 독립적. *ref) white noise: 통계적, 기술적 분석이 가능한 noise

### 정상성 Stationary 이란 
시간의 흐름 lag 에 있어서 평균이나 분산이 바뀌지 않는 데이터. (시간 단위 별로 평균이나 분산이 모두 동일)


## Time Series Decomposition 시계열 분해
1) Trend 추세요인
2) Seasonality 계절 요인
3) Cycle/Residual 순환 요인 / 잔차 (불규칙 요인) 
가 어떻게 나오는지 분해 (시켜서 확인)




## ARIMA (Auto Regressive Integrated Moving Average Model)
#### AR (Auto Regressive Model) 자기회귀모델
p 시점 이전의 자료가 -> 현재 시점의 데이터에 영향을 주는 자기회귀모델

-. AR모델만 쓰는 경우: 외란 요인에 의해 불규칙한 변동이 나타나면 설명력이 떨어짐

-. ACF (Auto Correlation Function) 자기 상관 함수 : p 구간 내 데이터의 상관 관계

-. PACF (Partial Auto Correlation Function) 부분 자기 상관 함수 : 다른 시점의 데이터들은 제외하고 특정 시점의 관측치와 현재 시점의 관측치, 두 관측치 사이의 상관관계

#### MA (Moving Average Model) 이동평균모델
일정한 구간 데이터의 평균을 계산해, 미래를 예측하는 모델

-. 장점: 불규칙한 변동 제거가 가능 <- AR+MA 모델을 같이 쓰는 이유

#### Difference 차분
차분을 함으로서 non-stationary data를 stationary data 로 변환.

### ARIMA Model 적용 case
-. 단기 예측에 적합

-. 계쩔적 변동 요인 (주기적인 변동) 이 있는 경우.

-. sample size > 50

-. 정상적 데이터 stationary data로 만든 후에 적용

  -> 비정상적 데이터인 경우,
  
  --> 평균이 증가/감소 (시간에 따른 평균의 trend가 변화하는 경우), difference 차분 수행
  
  --> 분산이 증가/감소 (시간에 따른 분산의 trend가 변화하는 경우), lag 변환
  
## Metric 검증 지표 


 
## Time Series 모델 Python 구현 시 Tip
-. 대부분 library가 시계열 데이터를 {DataFrame: index = DateTime / feature = Y}인 형태로 받는다 (input)
  

###### 참고문헌 


https://blog.naver.com/data_station/222493262626
