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
시계열에 영향을 주는 일반적 요인을 분리하여 분석/해석

1) Trend 추세요인

2) Seasonal 계절 요인

3) Cyclical 순환 요인 

-------> 1,2,3 을 뺀 나머지 잔차 (불규칙 요인) 가 어떻게 나오는지 분해시켜서 확인

4) Irregular/Residual 불규칙 요인

```python
#data: y2 (datatype: Serie)
#Index: Date
#Value: y-values

#Index (Datetime)   Y
#2020-01-23         1.3569
#2020-01-24         2.5667
#2020-03-03         5.1235
#... 

import statsmodels.tsa.api.as tsa

#시계열 분해
model_series = tsa.seasonal_decompose(y2, model={옵션})

#시각화
fig = model_series.plot()
plot.show()
```


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

### ARIMA 모델
-. Model Parameter: Order (p,d,q)

  -> p: AR모델의 p값. p 구간 내 데이터 사이의 상관관계
  
  -> d: difference value 차분 값 (횟수)
  
  -> q: MA모델의 q값. PACF의 편상관계수 q값.

### SARIMA 모델
-. Trend 여부에 대해 ARIMA Model 생성 후, Seasonal에 대해 ARIMA Model을 추가로 수행

-. Model Parameter: Order (p,d,q) + Seasonal Order (P,D,Q,M). 계절성 배제한 order + 계절성을 고려한 order
  
  -> P: PACF 함수 기준으로 계절성 반복 수 (계절성 주기 패턴 파악). P 구간 내 데이터 사이의 상관관계
  
  -> D: 계절성 여부 확인 (0 or 1)
  
  -> Q: ACF 함수 기준으로 계절성 반복 수 (계절성 주기 패턴 파악). 주기의 패턴이 얼마나 반복 되는지.
  
  -> M: 계절성 주기 값

```python
import statsmodels.tsa.api as tsa

model = tsa.statespace.SARIMAX(y2, order = ({p,d,q}),
                                    seasonal_order = ({P,D,Q,M}),
                                    enforce_stationarity = False,
                                    enforce_invertibility = False)

# model fitting & result
results = model.fit()
results_AIC = results.aic
print(results.summary())

# plotting & diagnostics
results.plot_diagnostics(figsize = (16,8))
plt.show()

# prediction
pred = results.get_prediction(start = pd.to_datetime('2020-03-03'), dynamic = False)
# pred_mean_df = pd.DataFrame(pred.predicted_mean).reset_index()
y_forecasted = pred.predicted_mean
y_truth = y2['2020-03-03':]
mse = ((y_forecasted - y_truth) ** 2).mean()

# prediction - test
pred_uc = results.get_forecast(steps = 50)
pred_ci = pred_uc.conf_int() # 추정된 계수의 신뢰구간

# plotting prediction
ax = y2.plot(label='observed', figsize=(14, 7))
pred_uc.predicted_mean.plot(ax=ax, label='Forecast')
ax.fill_between(pred_ci.index,
                pred_ci.iloc[:, 0],
                pred_ci.iloc[:, 1], color='k', alpha=.25)
plt.legend()
plt.show()
```

## 결과 해석 
-. https://kayeekim.github.io/DAML-Metric/
### Metric 검증 지표 
#### AIC, BIC (+ HQIC) 로 모델 적합도 평가 가능
-. 값이 낮을 수록 좋은 모델

-. likelihood 활용한 값.

### 검정 (통계적 가설 검정)
#### Ljung-Box Test 륭 박스 검정
-. ARIMA 모델의 회귀계수 도출에 대한 p-value 검정

-. 일정 기간의 관측치와 ARIMA 모델의 회귀계수가 얼마나 상관성이 있는가. ( h0: 일정기간 동안 관측치가 랜덤이고 독립적이다. )

  -> H0 (e.g. P-value > 0.05. *p-value = P>|z|*): 데이터가 상관 관계를 나타내지 않는다 (상관계수 = 0)
  
  -> *** H1 (e.g. P-value < 0.05): 데이터가 상관 관계를 나타낸다 (상관계수 != 0). 회귀 계수가 유의미하다. ***

  -> * P-value: 귀무 가설이 참일 확률
  
-. 모델 잔차 residual 에 대해서도 Ljung-Box Test 수행 가능 *참고. 단 이 경우, ARIMA 모델 자체가 회귀분석의 잔차는 정규성을 띈다는 가정을 어느정도 무시하고 진행하는 경우가 많기 때문에, 실무에선 크게 중요하게 여기지 않는 경우가 있음.
  
#### Jarque - Bera Test 쟈크베라 검정
-. 표본 데이터 (sample 데이터) 의 왜도 (skew; 얼마나 한쪽으로 쏠려있는지) 와 첨도 (kurtosis; 얼마나 뾰족한지) 가 정규분포와 일치하는지 검정

-. ARIMA 모델의 Residual 잔차의 분포가 정규분포를 따르는지에 대한 검정

  -> ** H0: 해당 잔차는 정규분포를 띈다 **
  
  -> H1: 해당 잔차는 정규분포를 띄지 않는다.

*참고. ARIMA 모델 자체가 회귀분석의 잔차는 정규성을 띈다는 가정을 어느정도 무시하고 진행하는 경우가 많기 때문에, 실무에선 크게 중요하게 여기지 않는 경우가 있음.

#### Heteroskedasticity Test 이분산 검정 
-. ARIMA 모델의 Residual 잔차의 분산에 대한 이분산 검정.

*참고. ARIMA 모델 자체가 회귀분석의 잔차는 정규성을 띈다는 가정을 어느정도 무시하고 진행하는 경우가 많기 때문에, 실무에선 크게 중요하게 여기지 않는 경우가 있음.


 
## Time Series 모델 Python 구현 시 Tip
-. 대부분 library가 시계열 데이터를 {DataFrame: index = DateTime / feature = Y}인 형태로 받는다 (input)
  

###### 참고링크


https://blog.naver.com/data_station/222493262626
