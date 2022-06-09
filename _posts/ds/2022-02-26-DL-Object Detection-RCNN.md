---
layout: post
title:  "[DL] Object Detection - R-CNN 계열 모델 (R-CNN, Fast R-CNN, Faster R-CNN)"
excerpt: "Object Detection - R-CNN 계열 모델 역사 및 특징"

categories:
  - ds
tags:
  - [DL]

toc: true
toc_sticky: true
 
date: 2022-02-26
last_modified_at: 2022-06-09

---

## Summary 
Object Detection (객체 검출, OD) 문제는 대표적인 Computer Vision Technique 중 하나이다. 
대표적인 OD 모델로 YOLO, R-CNN 계열 모델 등이 있다.

Object Detection 이 무엇인지 알기 위해, 
* 먼저 Image 분석 유형을 정리해 보고
* Object Detection 기술에서 채택한 방법 (2-stage vs 1-stage 방식)
* 대표적인 OD 모델인 R-CNN 계열 모델이 등장한 순서와 특징을 알아본다 (R-CNN, Fast R-CNN, Faster R-CNN).

# What is a Object Detection?
## Image 분석 유형
* Classification: 이미지 한장 당 Single Object 에 대한 classification 수행
* Classification + Localization: 이미지 한장 당 Single Object 에 대한 classification & 위치 탐색 수행
* **Object Detection**: 이미지 한장 당 Multiple objects 검출을 가능하게 하며, 각 사물별 classification & 위치 탐색 수행
(각 사물의 instance 를 bounding box 로 구분)
* Instance Segmentation: 이미지 한장 당 Multiple objects 검출을 가능하게 하며, 이미지 내 pixel 별로 classification 수행
(각 사물의 instance 를 pixel 단위로 구분)

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

----
# R-CNN 계열 모델 
![image](https://user-images.githubusercontent.com/98376833/157602170-f1d68c01-2c20-44be-889b-e5675ee81cd1.png)
_*모델 등장 순서: R-CNN -> Fast F-CNN -> Faster R-CNN_

## R-CNN (CVPR, 2014)
* (0) Input Image -> (1) Extract region proposals (~2k (2,000장)) -> (2) Compute CNN features -> (3) Classify regions (+ bbox regression)
* (1) Region proposal 방법: Input image 로부터 Selective Search 수행 - 2000개의 region proposal 을 찾음. 
    * 여기서 수행되는 Selective Search 알고리즘은 (딥러닝 연산이 아닌 기존 영상처리 기반의) CPU 연산으로 진행됨
* (2) Feature Extractor (Detection Network): 
    * Object Recognition을 위해 딥러닝 (CNN) 연산 활용
    * (1)에서 찾은 Region proposal 부분 (각 객체에 대한 후보 bbox들; candidate bboxes) 에 대해 Warp: Crop & Resize 한 후 이를 CNN의 input으로 사용 
        * Warp 하는 이유? 동일 사이즈로 만들어 주기 위해.  
        * R-CNN Case: Convolution Layer 의 입력에서부터 동일한 Input Size 로 넣어주어서 Output Size 를 동일하게 한다.
    * R-CNN (2014) Case: 개별 Region Proposal (a Candiidate bbox) 마다 CNN 수행 
* (3) Classification & Regression : Support Vector Machine / Bounding Box Regression
    * (2) 에서 얻은 각각의 Convolution 결과에 대해 Classification & Regression 을 진행하여 
    * 해당 candidate box 내 객체 존재 여부, bbox information (BBox 크기, 위치 재조정 여부) 을 얻는다 
    * R-CNN Case: Classifier 로 Linear SVM 사용
        *  SVM은 CNN으로 부터 추출된 각각의 Feature Vector 들의 점수를 Class 별로 매기고, 객체 여부 & 객체라면 어떤 클래스 객체인지 판별하는 역할을 한다.

### R-CNN (2014) 특징
* CNN으로 각 Region 의 클래스 분류 제안. 처음으로 DL 분야를 OD 에 적용

### R-CNN (2014) 한계점
* 전체 프레임 워크를 End-to-End로 학습 불가 - 단순히 컴포넌트 여러개가 함께 진행되는 형태. 
    * 1) Selective Search (ROI 추출), 2) CNN, 3-1) SVM, 3-2) Bounding Box Regression 각각의 컴포넌트가 다단계로 이루어져 한 번에 학습되지 않는다.
        * 1) 입력 이미지에 대해 CPU 기반의 Selective Search 진행
        * 2, 3) CNN, SVM, Regressor 모듈이 모두 분리되어 있음. 즉, SVM과 Bounding box regression 결과로 CNN 업데이트 불가
    * 따라서 Global Optimal Solution 을 찾기 어려우며, 속도 또한 느림
* 모든 RoI (Region of Interest) 를 CNN에 input으로 넣음. 즉 이미지 한장 당 2000번의 CNN 연산 필요
* 이러한 R-CNN의 단점을 해결하기 위해 Fast R-CNN, Faster R-CNN 등 후속 연구가 진행되었다.

### 동작 과정
![image](https://user-images.githubusercontent.com/98376833/157610885-98815c51-6845-4620-b2ac-bd92fc68f4a4.png)

1. (RoI 추출) Selective Search 방법으로 2000개의 RoI (Region of Interest) 추출  
2. 각 RoI 에 대해 동일한 크기의 입력 이미지로 변경 (warping 수행)  
3. (Feature 추출) CNN 활용. input -Warped Image / output - image의 feature  
4. (Classification) 각 클래스에 대해 _독립적_ 으로 훈련된 binary SVM 사용. input - image feature / output - 클래스 분류 결과  
5. (Regression) Regressor 활용. input - image feature / output - bounding box 예측 결과  
![image](https://user-images.githubusercontent.com/98376833/157611907-3e36d3f6-97f2-4062-a738-0595b5203b17.png)

![image](https://user-images.githubusercontent.com/98376833/157612037-a9128c9f-6e4b-4c2a-accf-ff849d1931b5.png)



## Fast R-CNN (ICCV, 2015)
* (1) Region proposal 방법: Input image 로부터 Selective Search 수행 (R-CNN과 동일, CPU 연산)
* (2) Feature Extractor (Detection Network): Input image를 CNN의 input 으로 사용 + ROI pooling ( (1) region proposal 수행 정보 기반) 을 통해 추출된 feature에게 물체의 위치정보 부여 (R-CNN과의 차이점)
* (R-CNN과 비교) 동일한 Region proposal 을 이용하지만, 이미지를 *한번만 CNN에 넣어* Feature Map 생성

### 특징
-. Feature Extraction ~ RoI Pooling ~ Region Classification ~ Bounding Box Regression 까지 End-to-End 학습 가능.

-. 즉, Selective Search 를 제외한 나머지 부분을 end-to-end 로 묶어서 학습 가능. R-CNN 에 비해 성능 개선

### 한계점
-. 여전히 Selective Search 부분은 CPU 에서 분리되어 수행 - CPU에서 진행되는 부분이 속도의 bottleneck 으로 존재

### 동작 과정  
![image](https://user-images.githubusercontent.com/98376833/157612592-c85eae46-8064-4451-811e-c315e2a0af55.png)

1. (RoI 추출) Selective Search 방법으로 2000개의 RoI (Region of Interest) 추출  
2. (Feature 추출) CNN 구조 활용. input - "원본" Image / output - image의 feature. 
*이 때 feature map 의 정보는 원본 이미지에서의 위치에 따른 정보를 포함하게 됨  
3. (RoI pooling) feature map 으로 RoI projection 을 시킨 후 RoI pooling을 거침. 
이 과정을 통해 feature map 상에서 사물이 존재할만한 위치의 필요한 정보만을 추출할 수 있게 됨 
4. (Classification & Regression) softmax, bbox regressor 사용  

-. 2. 3. 4. 과정은 end-to-end 프레임워크 안에서 진행

![image](https://user-images.githubusercontent.com/98376833/157612838-ad1eb705-7a28-46cc-b8fb-5bab33d23acd.png)
_ROI Pooling Layer_


### Faster R-CNN (NIPS, 2015)
* (1) Region proposal 방법: Input image 로부터 Region Proposal Network (RPN) 모델을 사용하여 Region proposal을 찾음. 
RPN 모델은 DL구조를 띄며 GPU 연산 사용이 가능 (R-CNN, Fast R-CNN과의 차이점). 
* (2) Feature Extractor: Detection Network 구조는 Fast R-CNN과 동일
* RPN 모델을 제안함으로서 (GPU 연산 가능), Selective Search (CPU 연산) 의 시간적인 문제를 해결함'
* 이 RPN 모델 또한 학습이 필요한데, 별도의 학습과정을 거치는 것이 아닌 End-to-End 방식으로 학습시킨다

### 특징
-. RPN이 제안되어, 전체 프레임워크가 end-to-end로 학습이 가능 (성능 개선). 

-. RPN이 GPU 연산이 가능하다는 점에서 속도 또한 개선

### 한계점
-. 모델 구조가 복잡하다는 한계는 여전히 존재 (자잘한 Technique 조절이 필요)

-. Region Classification 을 진행할 때, 각각의 서로 다른 region 에 대해 특징 벡터 (feature vector) 가 개별적으로 구분되어 FC layer 로 forwarding 되어야 함 

### 동작 과정
![image](https://user-images.githubusercontent.com/98376833/157615048-16e5c2da-10ef-4cda-aadc-117b8aac348e.png)

1. (Feature 추출) VGG 네트워크 구조 활용. input - "원본" Image / output - image의 feature. 
2. (RoI 추출) RPN 네트워크 활용. input - feature 
3. (Detect - Classification & Regression) Classifier (Dector Network) 사용. Fast R-CNN 구조와 동일.
input - RPN 네트워크 결과 OR Feature 추출 네트워크 결과에서 RoI pooling이 진행된 feature  

-. 전 과정은 end-to-end 프레임워크 안에서 진행. 모델 학습 시 RPN 네트워크와 Classifier 를 서로 번갈아가며 alternative 하게 학습 수행

#### RPN Network 동작 과정
![image](https://user-images.githubusercontent.com/98376833/157615258-dfe9e854-b289-4f3d-911e-e1107fc988fe.png)  
_RPN Network_

* input - feature map / output - 물체가 있을 법한 위치 (RoI) 예측
* 다양한 형태와 크기를 가지는 사물을 인식할 수 있도록, k 개의 서로다른 size 를 가지는 anchor box 사용
* sliding window 를 거쳐 각 위치에 대한 regression / classification 수행
* RPN에서 수행되는 classification 은 사물의 클래스 분류가 아닌 *사물인지 아닌지에 대한 여부 (binary)* 를 분류함


----
# Object Detection 에서 사용되는 기본 IDEA
## Region Proposals
* 물체가 있을 법한 위치를 찾는 과정
* ~ RoI 추출 ~ Candidate bboxes 추출
#### (a) Sliding Window 방식  
![image](https://user-images.githubusercontent.com/98376833/157607446-bde9bc2a-b6ab-4bb0-8053-08cef71a57ce.png)

* Faster R-CNN 에서 채택 (단, Faster R-CNN 에서 진행되는 RPN의 경우 GPU 에서도 연산수행이 가능하도록 제안되었음)

#### (b) Selective Search 방식   
![image](https://user-images.githubusercontent.com/98376833/157607633-39c827ea-07e0-4138-91b5-7c2c8ea7cd51.png)

* R-CNN, Fast R-CNN 에서 채택
* 기본적으로 CPU 연산으로 돌아가도록 library 가 작성되어 있음
* Selective Search 작동 원리
    * Step 1) Segmentation 이미지 획득
        * 기존 학습된 혹은 이미지 처리 모델을 이용하여 이미지 Segmentation 수행
        * 이 때 사용된 segmentation 방법은, 기존의 영상처리 방법을 활용  
    * Step 2) 반복적으로 작고 비슷한 Segmentation 영역을 점점 크게 하나로 합침
    * Step 3) Step 2을 통해 추려진 Segmentation 영역을 바탕으로 -> Region Proposal (candidate bboxes 제안) 
* R-CNN (2014) 에서 Selective Search 를 다루는 법
    * Step 1) 색상, 영역 크기 등을 이용해 Non-Object Based Segmentation 을 수행한다
        * Step 1 을 통해 가장 많은 Small Segmented Area 들을 얻을 수 있다.
    * Step 2) Bottom-up 방식으로 Small Segmented Area 들을 합쳐서 더 큰 Segmented Area 를 생성한다
    * R-CNN 에서는 위 두 step 을 반복하여 최종적으로 2000개의 Region Proposal (candidate bboxes) 을 생성한다. 

## 정확도 측정 방법
### 성능 평가 Metric: Precision & Recall
* Precision: 모델이 positive 로 올바르게 판단한 물체 수 (TP) / 모델이 positive 로 예측한 전체 물체 갯수 (TP+FP)
* Recall: 모델이 positive 로 올바르게 판단한 물체 수 (TP) / 실제 (Ground Truth) positive 인 물체 갯수 (TP + FN)

![image](https://user-images.githubusercontent.com/98376833/157608649-e4eec2d7-0a56-4c15-a118-0528f33ee44a.png)

* Average Precision: Precision 과 Recall을 함께 보기 위한 방법 중 하나. confidence에 따라 그려지는 단조 감소 그래프로 넓이 계산     
![image](https://user-images.githubusercontent.com/98376833/157608828-969dfe7e-995c-4a7b-a547-ef0b43807ae4.png)

### TP, FP 결정 방법: Intersection over Union (IoU)
* Interaction (교집합) over (나누기) Union (합집합) *참고: over는 분수를 표현할 때 사용
* OD 모델의 bounding box와 Ground Truth 의 bounding box 비교  
![image](https://user-images.githubusercontent.com/98376833/157609184-07839865-eee7-4aa3-8b86-67f2a364d928.png)

*참고) NMS (No Maximum Suppression) : 여러 개의 bounding box 가 겹쳐져 있는 경우에 (같은 class로 분류되면서 겹쳐 있다면) , 하나로 합치는 방법


----
해당 블로그는 개인적으로 정리한 글이며, 나동빈님의 '객체 검출(Object Detection) 딥러닝 기술: R-CNN, Fast R-CNN, Faster R-CNN 발전 과정 핵심 요약' 영상을 참고하였습니다.

#### Reference
객체 검출(Object Detection) 딥러닝 기술: R-CNN, Fast R-CNN, Faster R-CNN 발전 과정 핵심 요약: https://www.youtube.com/watch?v=jqNCdjOB15s
