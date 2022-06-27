---
layout: post
title:  "[DL] Object Detection - YOLO 계열 모델"
excerpt: "Object Detection - YOLO 계열 모델 역사 및 특징"

categories:
  - ds
tags:
  - [DL]

toc: true
toc_sticky: true
 
date: 2022-06-09
last_modified_at: 2022-06-27

---

## Summary 
* Object Detection (객체 검출, OD) 문제는 대표적인 Computer Vision Technique 중 하나이다. 
* 대표적인 OD 모델로 YOLO, R-CNN 계열 모델 등이 있다.
* Object Detection 이 무엇인지 알기 위해, 
    * 먼저 Image 분석 유형을 정리해 보고
    * Object Detection 기술에서 채택한 방법 (2-stage vs 1-stage 방식)
    * 대표적인 1-stage OD 모델인 YOLO 계열 모델이 등장한 순서와 특징을 알아본다 (YOLOv1, YOLOv2, YOLOv3) 

----
# YOLO 계열 모델 
* YOLO: You Only Look Once
_*모델 등장 순서: YOLO-v1 (2016) -> YOLO-v2 (2017) -> YOLO-v3 (2018) -> YOLO-v4 (2020) -> YOLO-v5 (2020)_

### YOLO-v1 vs YOLO-v2 vs YOLO-v3 구성요소 
* T.B.D

### (Reference) R-CNN vs Fast R-CNN vs Faster R-CNN 구성요소
* R-CNN
    * ROI (Selective Search) -> Conv (input: Each ROI) -> Support Vector MAchine / Bounding Box Regression 
* Fast R-CNN
    * ROI (Selective Search) and _Conv (input: Original Input Image)_ -> _ROI Pooling Layer_ -> Softmax, Bounding Box Regression
* Faster R-CNN
    * Conv -> _RPN (Region Proposal Network)_ -> ROI Pooling Layer -> Softmax, Bounding Box Regression 

---

## YOLO-v1 (2016)
* ![image](https://user-images.githubusercontent.com/98376833/172863925-54394af4-89a6-4bda-a52f-f0d665fe797f.png)
* YOLO-v1 (2016) 특징
    * 이미지를 일정 크기의 그리드 셀로 나누어 예측 
    * (A) Bounding Boxes + Confidence, (B) Class Probability Map, 이 두 개가 동시에 진행되며 (1-stage) 합쳐져서 output 이 tensor 형태로 나오게 된다.
    * Bounding Boxes + Confidence
        * 각 그리드 셀당 2개의 Bounding Box (BBox) 를 그리면서 구역을 예측
        * 각 Bounding Box 는 5개의 정보를 예측: [x, y, w, h, conf]
            * x: 객체 중점의 x 좌표
            * y: 객체 중점의 y 좌표
            * w: Bounding Box 너비
            * h: Bounding Box 높이
            * conf (confidence) : 해당 위치에 객체가 있을 확률 
    * Class Probability Map
        * 그리드 한 셀 (각 그리드 셀 당) 당 어떤 클래스에 속하는지 예측 (예측 시, 한 셀당 하나의 클래스에 속하는 것으로 할당)
* (0) Input Image -> (1) BackBone 모델 구조 (GoogleNet 사용) + 추가 conv layer -> (2-A) Bounding Boxes + Confidence: (2-A-1) FC layer 로 부터 Output 도출 (Candidate bboxes) & (2-A-2) NMS and (2-B) Class Probability Map
* (2) Classification & Regression: (2-A) Bounding Boxes + Confidence 도출 + (2-B) Class Probabiltiy Map 도출
    * (2-A, 2-B) Output : output의 한 셀 (1*1*k) = 그리드 한 셀에 대한 정보 [첫번째 bounding box 정보 (x, y, w, h, conf) + 두번째 bounding box 정보 (x, y, w, h, conf) + 클래스에 대한 정보 (클래스 수만큼 존재)]
    *  if # of classes = 20 and grid size = 7*7,
        * 총 98 (7*7*2) 개의 Bounding Box 가 물체의 위치와 20개의 클래스를 분류. (7*7*2) = (7*7) (grid size) * 2 (grid cell 당 2개 bbox 정보 예측) 
        * output의 한 셀은 30차원으로 이루어져 있음 (5 (첫번째 bbox 정보) + 5 (두번째 bbox 정보) + 20 (클래스수))
    * (2-A-2) NMS (Non-Maximum Suppression)
        * 여러 candidate bboxes 중 NMS 를 거쳐 가장 객체를 잘 나타내는 Bbox 하나를 선택  
 * (2-A-2)의 NMS를 통해 객체를 가장 잘 나타내는 bbox 하나가 선택되면, Class Probability Map 을 통해서 그 객체가 어떤 객체인지 판별 
    
### 동작 과정
![image](https://user-images.githubusercontent.com/98376833/172869874-b3434270-0c18-45c1-9df6-7617e08fb35e.png)

### YOLO-v1 (2016) 한계점
* (1) 객체 검출의 시간은 빠르나 성능이 떨어짐. 
    * 특히 작은 물체에 대한 성능이 좋지 않은 경우가 많았다.
* (2) 각 셀에 대해서 Bounding Box 로 단순 예측을 진행한다.
    * Anchor Box 등 별도의 박스를 사용하지 않는다.
* (3) 하나의 셀에서 하나의 객체 (Object) 만 예측한다.
    * 겸친 객체는 검출하지 못한다.
* (4) Bounding Box 가 위치를 제대로 잡지 못하는 경우도 있다.
* (2, 3, 4) 를 개선한 모델이 YOLO-v2 (2017)

---

## YOLO-v2 (2017)
* YOLO-v2 (2017) 특징
    * 성능과 속도를 모두 개선
    * 9000 개의 Object Category 를 검출하는데 성공한 YOLO9000 (2017)도 함께 소개 
    * YOLO-v1 에 비해 각종 모듈이 추가
        * ![image](https://user-images.githubusercontent.com/98376833/172871781-22e01cd6-9743-487c-9345-b17e8e2505f6.png)
    * 1단계 학습 (Pre-training) 과 2단계 학습 (Fine-tuning) 으로 이루어져 있다: YOLO-v2 에서는 Pre-training 을 활용한다.
        * 1단계 학습 (Pre-training) 
            * CNN 기반 Darknet-19 모델을 대량의 이미지를 수집한 ImageNet 데이터를 활용하여 사전에 학습시켰다.
            * 이 사전 학습을 통해 CNN 이 이미지 특징을 추출할 수 있도록 도와준다.
            * 참고: YOLO-v1 에서는 사전학습된 모델을 쓰지 않고, 해결할 데이터셋으로 바로 Scratch
        * 2단계 학습 (Fine-tuning) 
            * Pre-trained Darknet-19 모델의 마지막 layer 제거 후, Object Detection 을 위한 Module 을 추가한다.
            * 이 최종 네트워크로부터 경계 박스와 object detect 에 대한 학습을 시작한다
    * 앵커 박스 (Anchor Box) 를 도입한다 (Prior-Knowledge 부여)
        * 참고: YOLO-v1 에서는 Anchor Box 를 쓰지 않고 각 그리드 셀에 대해서 Classification 을 수행했다.
    * YOLO-v1의 마지막 Fully-connected layer 2개를 Convolution Layer 로 대체하였다
        * ![image](https://user-images.githubusercontent.com/98376833/172884463-3ca54b7e-deb1-46e1-87a3-ceb00186a1a4.png)
* (0) Input Image -> (1) "Pre-trained" Model (Darknet-19 사용) + Fine-tuning (추가 conv layer) -> (2-A) Bounding Boxes + Confidence: (2-A-1) FC layer 로 부터 Output 도출 (Candidate bboxes) & (2-A-2) NMS and (2-B) Class Probability Map
* (2) Classification & Regression: (2-A) Bounding Boxes + Confidence 도출 + (2-B) Class Probabiltiy Map 도출
    * (2-A, 2-B) Output : output의 한 셀 (1*1*k) = 그리드 한 셀에 대한 정보 [첫번째 Anchor box 정보 (x, y, w, h, conf, 클래스 정보 (클래스 수만큼 의 차원을 가짐)) + 두번째 Anchor box 정보 + ... ]
        * (YOLO-v2의 Output 차원수) (grid size) * (n_anchor boxes * (5 + n_classes)) 
        * (YOLO-v1) (grid size) * (5 + 5 + n_classes). *remind) YOLO-v1 에서는 셀당 2개의 bbox로 예측
    
### 동작 과정
* YOLO-v2 (2017) 도입 Technique: Anchor Box
* Anchor Box 도입 방법: Dimension Cluster, Direct Location Cluster
* Passthrough Layer
    * (YOLO-v1) CNN Architecture 의 마지막 Feature Map 만 사용하여 객체 탐지.
        * 작은 물체에 대해 정보가 사라진다는 비판이 있었다
    * (YOLO-v2) 상위 Layer 의 Feature Map을 하위 Layer 의 Feature Map 에 합쳐주는 Passthrough Layer 를 도입했다.
        *![image](https://user-images.githubusercontent.com/98376833/172883832-b2809c91-e66a-4b6b-9c99-e98bbd335744.png)
* Prediction 전 FC layer 를 Convolution layer 로 대체
    * (장점) FC Layer 를 사용하지 않음으로써, Input Image 의 해상도에서 비교적 자유로워 졌다.
* 작은 물체들을 잘 잡아내기 위해 YOLOv2는 여러 Scale 의 이미지를 Training 에 입력으로 활용했다
    * FC Layer 를 사용하지 않기 때문에, YOLOv1과 달리 Scale의 제약이 감소
    * YOLO-v2 Case: 학습 시에 320, 352, .., 608 과 같이 32픽셀 간격으로 매 10 batch 마다 입력 이미지의 해상도를 바꿔주며 학습을 진행하였다
* 모델 경량화
    * DarkNet 제안하여 도입
        * Max Pooling 대신 Convolution 연산을 늘림   
        * 마지막 단계에서 FC Layer 를 제거하고, Convolution 연산으로 대체함으로써 파라미터 수를 줄임
        * 결론적으로, YOLO-v1 에서 사용하던 GoogLeNet 기반보다 경량화된 CNN 아키텍처를 사용함으로써 속도 개선

#### Anchor Box 도입
* Anchor Box 사용 이유
    * 사전에 크기와 비율이 모두 결정된 박스를 전제하고, 학습을 통해서 이 박스의 위치나 크기를 세부 조정
    * (장점) 아예 처음부터 [x, y, w, h] 를 찾는 것보다 안정적으로 학습이 가능하다 
        * In Paper, Anchor box 를 적용함으로서 Accuracy 5% 상승
 
##### Dimension Cluster (차원 클러스터)
* Anchor Box 를 찾기 위해 **K-means Clustering** 도입
* YOLO-v2 Case: 저자는 COCO Dataset 의 Bounding Box 에 K-means Clustering 을 적용한 결과, Anchor Box 를 **5개**로 설정했을 때 Precision 과 Recall 모든 측면에서 좋은 결과를 낸다고 제안

##### Direct Location Cluster (직접적인 위치 예측)
* ![image](https://user-images.githubusercontent.com/98376833/172881578-2ea0ef1b-9d31-4de1-8c52-939c69a456e3.png)
* Anchor Box 를 따라, 하나의 셀에서 5차원 벡터 (t_x, t_y, t_w, t_h, t_o) 로 이루어진 BBox 예측 하는 기능 도입
* 위 5차원 벡터 (t_x, t_y, t_w, t_h, t_o)를 통해 현재 앵커박스에서 조절이 필요한 위치 정보(b_x, b_y, b_w, b_h) 계산
    * (위치에 대한 offset) b_x, b_y: 사전에 정의된 anchor box 대비 Bounding Box의 중심점을 얼마나 옮겨야 하는지 (중심점에 대한 Offset)
    * (크기 scale 조절) b_w, b_h: 사전에 정의된 anchor box 의 크기를 얼마만큼의 비율로 조절해야하는지   
        * 지수 승을 통해 예측한다.
    * 단, Bounding Box 의 중심은 항상 grid 셀 안에 있어야 한다.
        * Why? 이미 각 셀마다 중심점 예측을 수행하기 때문 (~) 각 셀마다 예측한 중심점 보유. 이 rule 을 깨지 않도록 하기 위함


### YOLO-v2 (2017) 한계점

---

## YOLO-v3 (2018)
* 원 paper: Yolov3: An incremental improvement (2018)
* YOLO-v3 (2018) 모델 Summary
    * 1) 네트워크 모델을 기존의 darknet-19 에서 darknet-53 으로 확장함에 따라 네트워크의 복잡성이 증가
    * 2) 기존 YOLO 모델과 비교하면 속도가 하락하였으나, 정확도가 급격히 상승함
    * 3) 객체탐지 (Object Detection ) 모델에서 굉장히 적극적으로 활용되고 이음. 
* YOLO-v3 (2018) 특징
    * 수행 시간은 조금 느려졌으나, 성능이 대폭 개선
    * (YOLO-v2 와 비교했을 때 신규 테크닉) Prediction Across Scale, Multi-label Classification 테크닉 도입
* (0) Input Image -> (1-A) Backbone 모델 (Darknet-53 사용) & (1-B) Scale 사용: Scale을 다르게 하여 (e.g. 작은 사이즈 이미지에서, 중간 사이즈 이미지에서, 큰 사이즈 이미지에서 Object Detection 수행) -> (2) Bounding Boxes + Confidence: (1-B)의 Scale 별 결과들을 취합해서 최종적으로 bbox에 대한 output 과 객체에 대한 정보출력
    * Darknet-53: YOLO-v2에서 사용한 Darknet-19 확장
    * Scale 1: Scale이 작은 그림 (사이즈를 작게 한 이미지) 에서는 큰 물체를 인식
    * Scale 2, 3: Scale을 크게해서 본 그림 (사이즈를 크게 한 이미지) 일 수록 점점 작은 물체를 예측 (인식)
* Tensor 형태
    * (YOLO-v3의 Output 차원수) (grid size) * (n_anchor boxes * (5+n_clasess)) (same as YOLO-v2)     
        * 예시: 텐서 형태) (N*N) * (3 * ((4+1) + 80))
        * grid size : N*N
        * n_anchor boxes: BBox 수 (ex. 3)
        * 5 : bbox 박스 좌표 정보 (4개; offset: x, y, w, h) + Objectiveness Score (1개)
        * n_classes: 주어진 데이터의 class category 수 (ex. 80)

### 동작 과정
* **Yolo-v2 (2017) 와 같은 방식** 의 Dimension Cluster (차원 클러스터) 와 Direct Location Cluster (직접적인 위치 예측) 사용
    * Dimension Cluster: "K-means Clustering( Dimension Cluster )" 을 통해 Anchor Box 를 생성하여 Bounding Box 예측
    * Direct Location Cluster: Anchor Box의 중심 위치 (b_x, b_y) 및 너비와 높이 (b_w, b_h) 를 어떤 비율로 조절할지를 예측함 
* Prediction Across Scale 도입
* Multi-label Classification 도입

#### Prediction Across Scale
* 요약: Anchor Box 를 Scale 별로 나눠준다.
* 총 3개의 Scale을 활용하여, 각 Scale 당 3개의 Anchor Box 를 가짐. 
    * Anchor box: 훈련 데이터 셋을 바탕으로 K-Means Clustering 을 적용하여 (Dimension Cluster 테크닉), 
    * (YOLO-v3) 9개의 Anchor Box를 결정 -> 각 scale 당 3개의 Anchor Box 를 할당
    * ex) COCO 데이터에서의 Anchor Box 형태:
        * (10*13), (16*30), (33*23), (30*61), (62*45), (59*119), (116*90), (156*198), (373*326) 

#### Multi-label Classification 
* 각 box 는 Multi-label Classification 을 활용하여 Bounding Box 가 포함될 수 있는 Class 를 예측
    * Multi-label: 하나의 셀에 두가지 이상의 객체가 같이 있을 수 있게 허용
* Class Predictor: softmax 대신 logistic classifier를 사용 
    * 각각의 클래스에 대해서 예측 (softmax) -> 이진 분류 문제로 변화 (logistic)
* 장점: 사람이면서 여자인 경우와 같은 Multi-label 문제 해결 가능


### YOLO-v3 (2018) 한계점

----

#### Reference
* (이전 포스트) [DL] Object Detection - R-CNN 계열 모델 (R-CNN, Fast R-CNN, Faster R-CNN): https://kayeekim00.github.io/DL-Object-Detection-RCNN/
* (이전 포스트) [DL] Object Detection 개요와 작동원리: https://kayeekim00.github.io/DL-Object-Detection-What-is-a-Object-Detection/
