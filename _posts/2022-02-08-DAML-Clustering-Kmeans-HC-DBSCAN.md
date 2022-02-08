---
layout: post
title:  "[DA/ML] 군집화 Clustering - K-means, Hierarchical Clustering, DBSCAN"
excerpt: " 비지도학습 - Clustering - K-means, Hierarchical Clustering, DBSCAN"

categories:
  - Blog
tags:
  - [DA/ML]

toc: true
toc_sticky: true
 
date: 2022-02-08
last_modified_at: 2022-02-08
---

# Unsupervised Learning (USL) 비지도 학습
-. 데이터 label 이 없을 때

-. feature 들의 특징을 가지고 일정한 규칙을 찾아내는 ML기법

## K-means Algorithm
-. 주어진 데이터를 k개의 cluster 로 묶는 USL 알고리즘

-. 데이터의 특징만으로 비슷한 데이터들끼리 Clustering


### 장,단점
#### 장점
-. simple & fast. 계산량이 많지 않음.

#### 단점
-. k (클러스터 갯수) 설정 / 최초 중심점 (위치) 설정에 따라 결과가 크게 달라질 수 있음

-. 결과 해석이 쉽지 않음. 정답이 없기 때문에 결과 검증이 .


### Algorithm 수행 Process
#### 1) k값/최초 중심 설정 
-. k (클러스터링 갯수) 설정 / k개의 최초 중심값 설정

-. k 에 따라 모델의 성능 좌우.

-. 최초 중심점 설정 방법: a) random 설정, b) 사용자 설정, c) k-means++

-. k-means++? 최초로 설정된 중심점이 너무 가까운 경우, clustering 모델 비효율/좋지 않은 성능을 가져올 수 있음. 이를 보완하기 위해 k-means++ 는 최초 중심점을 가장 멀리 있는 점으로 설정. 최초 중심점들의 위치가 모여 있는 것을 방지.

#### 2) 데이터의 클러스터 결정 
-. 분류되지 않은 데이터의 cluster 설정. 각 cluster의 중심값과의 거리 distance 를 비교하여 가장 가까운 cluster로 귀속

#### 3) 클러스터 내 중심 이동 
-. 2)에서 각 cluster로 분류된 데이터들을 기반으로 중심점 재설정

-. 이전 cycle에서 설정된 각 cluster의 중심값을, 현 cycle 에서 각 cluster 로 분류된 데이터들의 중앙값으로 변경

#### 4) 최종 클러스터 결정 
-. 중심점이 변경되지 않을 때까지 step 2) ~ 3) 반복. 중심점이 변경되지 않는 시점이 오면, 해당 시점에서의 클러스터를 최종 클러스터로 결정.





###### 참고링크
실무에 활용하는 머신러닝

http://www.kocw.net/home/cview.do?mty=p&kemId=1380150
