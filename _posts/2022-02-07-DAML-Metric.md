---
layout: post
title:  "[DA/ML] 모델 평가 지표/관련 통계량 Metric"
excerpt: "Metric 개념 정리 - 모델 평가 지표/관련 통계량"

categories:
  - Blog
tags:
  - [DA/ML]

toc: true
toc_sticky: true
 
date: 2022-02-08
last_modified_at: 2022-02-08
---


## 통계 개념
### Likelihood 최대우도
-. 특정 데이터가 모수로부터 추출되었을 가능도

-. 특정 값에 대한 분포의 확률 추정 (p.d.f (연속 확률 밀도 함수; 확률분포도) 의 y-value)

e.g. 몸무게 50kg 학생이 확률분포 상에서 몇 %에 위치하는지.? (<=> (반대 개념) 상위 50%의 학생의 몸무게를 물어보는 것) 

### P-value & 유의 수준
-. p-value: 귀무 가설이 참일 확률

-. 유의수준

-. p-value < 유의수준 : h1 (대립가설) 채택.

## Metric 정보 기준으로 쓰이는 metric 정리
### AIC (Akaike Information Criterion) 아케이케 정보 기준
-. 데이터에 대한 모델의 상대적 품질 수준을 수치화

-. 값이 낮을 수록 모형 적합도가 높은 것을 의미. 값이 낮을 수록 잘 만들어진 모델이다. 

-. AIC = -2 ln (L) + 2 where L: Likelihood function, k: paramter)

-. likelihood 함수를 바탕으로 계산되는 값

### BIC (Bayes Information Criterion) 베이지안 정보 기준
-. 데이터에 대한 모델의 상대적 품질 수준을 수치화

-. 값이 낮을 수록 모형 적합도가 높은 것을 의미. 값이 낮을 수록 잘 만들어진 모델이다. 

-. BIC = -2 ln (L) + log(n)p 

-. 변수 (설명변수) 수가 많은 경우, AIC 에 더 많은 panelty 부여. 변수 수에 따른 패널티 부여 가능 (AIC 보완).  --> 변수 갯수가 많으면 AIC 값 대비하여 BIC 값이 높게 나올 수 있음.

### HQIC (Hannan-Quinn Information Criterion) 헤넌 퀸 정보 기준
-. 데이터에 대한 모델의 상대적 품질 수준을 수치화

-. 값이 낮을 수록 모형 적합도가 높은 것을 의미. 값이 낮을 수록 잘 만들어진 모델이다. 

-. HQIC = -2 ln (L) + 2k ln(ln(n))

-. AIC, BIC 값 대안으로 사용. likelihood에 대한 미세 조정 함수로 보통 사용.


###### 참고링크

https://blog.naver.com/data_station/222493262626
