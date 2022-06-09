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
last_modified_at: 2022-06-09

---

## Summary 
* Object Detection (객체 검출, OD) 문제는 대표적인 Computer Vision Technique 중 하나이다. 
* 대표적인 OD 모델로 YOLO, R-CNN 계열 모델 등이 있다.
* Object Detection 이 무엇인지 알기 위해, 
    * 먼저 Image 분석 유형을 정리해 보고
    * Object Detection 기술에서 채택한 방법 (2-stage vs 1-stage 방식)
    * 대표적인 OD 모델인 R-CNN 계열 모델이 등장한 순서와 특징을 알아본다 (R-CNN, Fast R-CNN, Faster R-CNN).

----
# YOLO 계열 모델 

_*모델 등장 순서: R-CNN -> Fast F-CNN -> Faster R-CNN_

### R-CNN vs Fast R-CNN vs Faster R-CNN 구성요소
* R-CNN
    * ROI (Selective Search) -> Conv (input: Each ROI) -> Support Vector MAchine / Bounding Box Regression 
* Fast R-CNN
    * ROI (Selective Search) and _Conv (input: Original Input Image)_ -> _ROI Pooling Layer_ -> Softmax, Bounding Box Regression
* Faster R-CNN
    * Conv -> _RPN (Region Proposal Network)_ -> ROI Pooling Layer -> Softmax, Bounding Box Regression 

---

## R-CNN (CVPR, 2014)




----

#### Reference
