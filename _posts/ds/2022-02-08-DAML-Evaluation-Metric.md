---
layout: post
title:  "[DA/ML] 모델 평가 Evaluation 와 지표/통계량 Metric"
excerpt: "Metric 개념 정리 - 모델 평가 지표/관련 통계량"

categories:
  - ds
tags:
  - [DA/ML]

toc: true
toc_sticky: true
 
date: 2022-02-08
last_modified_at: 2022-02-10
---

## ML 개념
### 과소적합 Underfitting and 과대적합 Overfitting
#### 과소적합 underfitting
-. 모델 학습 시, 충분하지 못한 특징만으로 학습. 

-. train/test dataset 모두 정확도가 낮을 경우, 과소 적합 의심

-. bias가 높음. 특정 feature 에만 bias되게 학습

-. 개선방법: feature 수 증가

#### 과대적합 overfitting
-. train dataset 에 대한 정확도는 높지만, test dataset 에 대한 정확도는 낮은 경우. 과대 적합 의심.

-. Variance 높음. train dataset 외에 데이터가 들어오면 정확도가 낮게 나옴

-. 개선방법: 1) train dataset 을 늘리거나, 2) feature 수를 감소.

----

## 통계 개념
### Likelihood 최대우도
-. 특정 데이터가 모수로부터 추출되었을 가능도

-. 특정 값에 대한 분포의 확률 추정 (p.d.f (연속 확률 밀도 함수; 확률분포도) 의 y-value)

e.g. 몸무게 50kg 학생이 확률분포 상에서 몇 %에 위치하는지.? (<=> (반대 개념) 상위 50%의 학생의 몸무게를 물어보는 것) 

### P-value & 유의 수준
-. p-value: 귀무 가설이 참일 확률

-. 유의수준

-. p-value < 유의수준 : h1 (대립가설) 채택.

---

## Metric: 정보 기준으로 쓰이는 metric 정리
### Confusion Matrix 혼동 행렬
|           |       |**예측결과**      |      |
|---|---|---|---|
|           |       |True              |False |
|**실제정답**|True  |True Positive (TP)|False Negative (FN)|
|            |False |False Positive (FP)|True Negative (TN)|

* True (맞췄는 지 여부) Positive (모델 예측 결과)

-. True Positive: 맞다고 올바르게 예측. True Negative: 틀리다고 올바르게 예측.

### Accuracy 정확도
-. 입력된 데이터셋에 대해 올바르게 예측한 비율

-. $ Accuracy = (TP+TN)/(TP+FN+FP+TN) $ 

### Precision 정밀도
-. 모델이 True 로 예측한 값이 실제로도 True일 비율.

-. 실제: False / 예측: True 라고 판단 (예측) 하면 안되는 경우 중요하게 사용. 

-. $ Precision = TP/(TP+FP) $

### Recall 재현도
-. 실제 True 인 값 중에서 모델이 True로 올바르게 예측한 비율

-. 실제: True / 예측: False 라고 판단 (예측) 하면 안되는 경우 중요하게 사용.

-. $ Recall = TP/(TP+FN) $

-. Precision and Recall 은 trade-off 관계

### F1-score
-. Precision, Recall 두 값을 조합하여 하나의 수치로 표현한 지표

-. 클래스 불균형 데이터일 경우, accuracy 평가는 왜곡된 성능평가로 이어질 수 있기 때문에 f1 score를 사용

-. $ F1 score = (2*precision*recall)/(precision + recall)

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

---

## Validation 검증방법
### K-fold cross validation
-. training set의 일부분을 validation set으로 활용. 이 때 k번의 검증 과정을 사용하며, training set의 모든 데이터를 한번씩 validation set으로 사용할 수 있게 함.

-. 최종 accuracy = Mean(1st~k-th 검증의 accuracy)

#### 장점
-. validation 결과가 일정 데이터에 편향되어 있지 않음. 모든 데이터에 대한 결과이므로 신빙성이 높음

-. 별도의 validation set 분리가 필요하지 않음.


---

###### 참고링크
https://blog.naver.com/data_station/222493262626
