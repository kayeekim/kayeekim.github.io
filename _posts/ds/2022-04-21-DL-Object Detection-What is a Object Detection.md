---
layout: post
title:  "[DL] Object Detection 개요와 작동원리"
excerpt: "Object Detection - 작동원리, OD 모델 종류"

categories:
  - ds
tags:
  - [DL]

toc: true
toc_sticky: true
 
date: 2022-04-21
last_modified_at: 2022-06-09

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
    * Computer Vision, Image Processing 에서 핵심 접근 중 하나
    * 다른 Computer Vision 분야에도 기반 기술로 활용
        * Object 간 관계 분석 등
    * 기계 학습 + 딥러닝 측면 모두 접근 가능
        * 참고: 2012년 이전까지는 영상 처리로 문제를 해결하려 했었다. 그 이후~최근까지 딥러닝 방법을 활용한 방법론들이 많이 제안되고 있다.
        * e.g. R-CNN (2013), OverFeat (ICLR' 14), Fast R-CNN (ICCV' 15), Faster R-CNN (NIPS' 15), OHEM (CVPR' 16), YOLOv1 (CVPR' 16), SSD (ECCV' 16), R-FCN(NIPS' 16), YOLOv2 (CVPR' 17), FPN:Feature Pyramid Net (CVPR' 17), RetinaNet (ICCV' 17), Mask R-CNN (ICCV' 17), YOLOv3 (arXiv' 18), RefineDet (CVPR' 18), M2Det (AAAI' 19)
    * 응용 분야: 자율 주행, Face Recognition, 제조업 - 불량 이미지 검출, 인체 행동 분석, 로보틱스, etc.

### Image 분석 유형
* Classification: 이미지 한장 당 Single Object 에 대한 classification 수행
* Classification + Localization (위치 탐지) : 이미지 한장 당 Single Object 에 대한 classification & 위치 탐색 수행
* **Object Detection**: 이미지 한장 당 Multiple objects 검출을 가능하게 하며, 각 사물별 classification & 위치 탐색 수행
(각 사물의 instance 를 bounding box 로 구분)
* Instance Segmentation: 이미지 한장 당 Multiple objects 검출을 가능하게 하며, 이미지 내 pixel 별로 classification 수행
(각 사물의 instance 를 pixel 단위로 구분

### Input and Output
* Input: Image, Output: (For each proposed region) Multi-Class Classification, Bounding Box Regression 
* Input Dataset 연습용 Sample (+찾는 법)
    * Pascal VOC 데이터셋: 차량, 비행기, 자전거, 보트 등 20 개의 카테고리 포함하여 사람이 annotation 해놓은 데이터셋
    * AI 허브 Vision 데이터셋: 과학기술정보통신부, NIA 한국지능정보사회진흥원이 공동 개발한 한국형 Vision 데이터셋
        * https://aihub.or.kr/aihub-data/vision/about 

### OD에서 Classification 과 Regression 의 역할
* (Output) Classifier: Bounding Box Define / Regressor : Bounding Box Size 최적화
    * Classification: 물체의 클래스 분류 
    * Regression: 이미지 내 사물이 존재하는 bounding box 위치를 예측


## Object Detection 기술에서 채택한 방법 (2-stage vs 1-stage)
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
* 장점: 속도가 빠르지만 / 단점: 정확도는 낮은 경우가 많다 (2-Stage Detector 대비).
* 대표적인 모델: YOLO 계열 모델

## Object Detection 평가방법
)

# Object Detection 문제에서 쓰이는 개념 
## Bounding Box 란?
* 객체의 위치를 Localization 하는 것: 객체의 위치가 어디인지, 객체의 크기가 어느 정도인지 
    * 객체의 위치와 크기를 알아내는데 활용
    * 보통은 직사각형 (3D의 경우 직육면체) 의 네모 박스를 활용

## IoU: Intersection of Union
* 예측한 Bounding Box (BBox) 와 실제 Bbox 가 얼마나 일치하는지 측정하기 위한 **평가 지표 (Metric)**
* IoU = Area of Overlap / Area of Union (<=>) 교집합 영역 넓이 / 합집합 영역 넓이
    * Area1: Ground-Truth Bounding Box (BBox)
    * Area2: Predicted BBox 
    * ![image](https://user-images.githubusercontent.com/98376833/172760598-7cc75b6c-8352-4409-a31b-e5878e92eda1.png)
* 객체 탐지기 (Object Detector) 의 정확도를 측정하는데 이용하는 평가 지표 (Metric)
    * 즉, 알고리즘이 출력한 예측 Bounding Box 는 IoU를 이용해서 평가될 수 있음
    * IoU = 1: Predicted BBox = GT BBox

## NMS: Non-Maximum Suppression 비-최대 억제
* 객체 탐지기 (Object Detector) 가 예측한 BBox 중 가장 정확한 BBox 를 선택하도록 하는 기법
* 여러 개의 BBox 중 가장 정확한 하나의 BBox 만을 선택

### NMS 를 사용하는 상황
* Input Image 의 그리드 (grid)가 19*19 로 이루어져 있다고 할 때,
* 가장 이상적인 상황
    * 가장 적절한 그리드 내에 객체별 하나의 중앙점 (Mid-Point) 가 있는 것
* 보통의 상황
    * 객체별 중앙점 (Mid-Point) 가 여러 개가 detect 되는 경우가 많음.
    * 이 때, 어떤 그리드의 중앙점을 선택해야할까?
*  --> 이 때 사용 하는 알고리즘이 Non-Maximum Suppression

### NMS 작동원리
* OD 결과로 나온 객체 BBox 정보와 해당 BBox 가 객체일 확률 (Probability) 을 활용


----

#### Reference
* (이전 포스트) [DL] Object Detection - R-CNN 계열 모델 (R-CNN, Fast R-CNN, Faster R-CNN): https://kayeekim00.github.io/DL-Object-Detection-RCNN/
* (Paper) Zou, Z., Shi, Z., Guo, Y., & Ye, J. (2019). Object detection in 20 years: A survey. arXiv preprint arXiv:1905.05055.
