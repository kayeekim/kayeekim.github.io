---
layout: post
title:  "[DA/ML] Recommandation System Overview"
excerpt: "Recommandation System Overview"

categories:
  - ds
tags:
  - [DA/ML] 

toc: true
toc_sticky: true
 
date: 2022-05-30
last_modified_at: 2022-05-30

---
# Summary

---

# 상품 추천 알고리즘이란?
* 내가 소비할 상품(콘텐츠) 를 골라주는 방법
* How?
    * Random Selection
        * 단점: 이미 소비를 한 상품을 중복 추천할 수 있음
    * Guided Random Selection
        * 소비를 안한 것 중에 아무거나 고르기
        * 단점: 좋아하지 않는 것을 추천할 수 잇음
    * Taste Selection
        * 소비를 안한 것 중에 좋아하는 것을 추천하기
        * 문제점: 좋아하는 것을 정의하는 것이 필요

### Taste Selection - 고객이 "좋아하는 것"을 정의하는 방법
* Public taste
    * 대중이 좋아하는 것 = 고객이 좋아하는 것
    * Rating based, Frequency based
* Personal taste
    * 고객이 이전에 (좋아하여) 보았던 것들과 비슷한 것. 고객이 이전에 보았던 것 = 고객이 좋아하는 것
    * Contents based Filtering (콘텐츠 기반 필터링), Collaborative Filtering (CF, 협업 필터링) 
        * Collaborative Filtering 종류:  Memory based - Item based CF, User based CF / Model based - Latent factor based CF)

#### Contents based Filtering (콘텐츠 기반 필터링) vs Collaborative Filtering (CF, 협업 필터링)
* Contents based Filtering (콘텐츠 기반 필터링)
    * 사용자 (User) 가 시청한 콘텐츠와 유사한 콘텐츠를 추천하는 방법이다.
    * 장점: 신규 콘텐츠 추천이 가능하다.
* Collaborative Filtering (CF, 협업 필터링)
    * 협업 필터링은 시청 기록 (과거 경험; 과거 기록) 이 **유사**한 사용자의 데이터를 기반으로 콘텐츠를 추천한다.
    * 취향이 비슷한 사용자를 찾기 위해 Cosine Similarity 계산값을 활용한다.
    
#### Memory Based vs Model Based
* Memory Based 추천 방법
    * 대표: Item-based Collaborative Filtering, User-based Collaborative Filtering
    * 메모리 기반의 추천 방법은 사용하는 데이터 (수집된 데이터) 에 비례하여 메모리가 사용됨
* Model Based 추천 방법
    * 대표: Latent factor based Collaborative Filtering (잠재 요인 협업 필터링)
    
##### Latent factor based Collaborative Filtering (잠재 요인 협업 필터링)
* 잠재 요인 협업 필터링은 SVD 를 통해 SVD 를 통해 user-item matrix 를 세 개의 matrix로 분해한다.
* 분해한 matrix 에서 중요도가 높은 t개의 값으로 효율적인 추천 알고리즘을 생성한다.
* 장점: User-item matrix 의 일부만 사용함으로써 User-based CF, Item-based CF 보다 효율적이다 
    * User-item matrix 의 일부만 사용한다고 해서, 추천 알고리즘의 성능이 떨어지는 것은 아니다.

---
## 상품 추천 알고리즘의 평가 방법 (Metric)
### Accuracy
* 추천 알고리즘의 정확도를 나타낸다.

### Precision, Recall and F1-measure (f1-score)
* Precision = TP/(TP+FP)
* Recall =  
* F1-score 는 precision 과 recall 의 조화평균이다.

### nDCG (normalized Discounted Cumulative Gain)
* 추천 순서를 고려하여 추천 성능을 평가하는 지표
* nDCG 를 사용하면 추천의 갯수나 랭킹 결과의 길이에 따른 차이를 고려하여 비교할 수 있다. 
* min, max 범위로 0~1 사이의 값을 가진다.

---
## 추천 알고리즘의 성능 향상 Tip
* k-fold cross validation 적용
    * train dataset 의 일관된 성능 검증이 가능하다.
* Hybrid 추천 알고리즘
    * cold start 문제를 해결 할 수 있다.
* 주어진 데이터를 기반으로 새로운 데이터가 왔을 때도 잘 예측하는 모델을 찾아라

---
#### Reference)

