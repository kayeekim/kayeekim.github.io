---
layout: post
title:  "[DL] What is a Object Detection"
excerpt: "Object Detection - 작동원리, OD 모델 종류"

categories:
  - ds
tags:
  - [DL]

toc: true
toc_sticky: true
 
date: 2022-04-21
last_modified_at: 2022-04-21

---

## Summary


---

# What is a Object Detection?
* (이전 포스트 보완) [DL] Object Detection - R-CNN 계열 모델 (R-CNN, Fast R-CNN, Faster R-CNN): https://kayeekim00.github.io/DL-Object-Detection-RCNN/
* Object Detection 작동 원리
* 2-stage, 1-stage Object Detection 방법 소개 (추가)
* 평가 방법
    * Intersection of Union (IoU)
    * Non-maximum Suppression (NMS)  

## Object Detection (객체 영역 탐지) 기술 작동원리
* 분류 + 위치 탐지 = 객체 탐지
* Classification + Regression - 모델 결과로 Classification + Bounding Box 출력
* 객체 탐지의 활용도
    * Computer Vision, Image PRocessing 에서 핵심 적븐 중 하나
    * 다른 Computer Vision 분야에도 기반 기술로 활용
        * Object 간 관계 분석 등
    * 기계 학습 + 딥러닝 측면 모두 접근 가능
    * 응용 분야: 자율 주행, Face Recognition, 제조업 - 불량 이미지 검출, 인체 행동 분석, 로보틱스, etc.

### Image 분석 유형
* Classification: 이미지 한장 당 Single Object 에 대한 classification 수행
* Classification + Localization (위치 탐지) : 이미지 한장 당 Single Object 에 대한 classification & 위치 탐색 수행
* **Object Detection**: 이미지 한장 당 Multiple objects 검출을 가능하게 하며, 각 사물별 classification & 위치 탐색 수행
(각 사물의 instance 를 bounding box 로 구분)
* Instance Segmentation: 이미지 한장 당 Multiple objects 검출을 가능하게 하며, 이미지 내 pixel 별로 classification 수행
(각 사물의 instance 를 pixel 단위로 구분

### Objection Detection 작동 원리
* 


## Object Detection 기술에서 채택한 방법 (2-stage vs 1-stage)
### OD에서 Classification 과 Regression 의 역할
* Classification: 물체의 클래스 분류 
* Regression: 이미지 내 사물이 존재하는 bounding box 위치를 예측

### 2-Stage Detector
* (1) Localization (물체 위치 파악) 와 (2) Classification (물체 구분) 문제를 **구분해서** 해결
* Image -> 1) Region proposals -> 2) Feature Extractor -> Classification & Regression
* (1) Region proposals: 위치에 대한 정보 제안
* (2) Feature Extractor: 각각의 위치에 대해서 feature를 추출
* 장점: 1-stage detector 방식에 비해 정확도는 높은 편이지만 / 단점: 속도는 느린 경우가 많다
* 대표적인 모델: R-CNN 계열 모델

### 1-Stage Detector
* Localization + Classification 문제를 **한번에** 해결
* Image -> Feature Extractor -> Classification & Regression
* 장점: 속도가 빠르지만 / 단점: 정확도는 낮은 경우가 많다.
* 대표적인 모델: YOLO 계열 모델

## Object Detection 평가방법
)


----

#### Reference
* (이전 포스트) [DL] Object Detection - R-CNN 계열 모델 (R-CNN, Fast R-CNN, Faster R-CNN): https://kayeekim00.github.io/DL-Object-Detection-RCNN/
