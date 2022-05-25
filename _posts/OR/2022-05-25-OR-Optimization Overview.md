---
layout: post
title:  "[OR] Mathematical Optimization (최적화) Overview"
excerpt: "Operations Research - 최적화 overview"

categories:
  - OR
tags:
  - [OR]

toc: true
toc_sticky: true
 
date: 2022-05-25
last_modified_at: 2022-05-25

---

## Summary
* 최적화 (수학적 최적화) 정의와 사용 이유를 다뤄본다. 
* 생산 시스템에 대해 알아본다.
* 
---

# 최적화 (수학적 최적화) Overview

## 최적화 (mathematical optimization) 정의
* 최적화(最適化, 영어: mathematical optimization 또는 mathematical programming)는 특정의 집합 위에서 **의된 실수값, 함수, 정수에 대해 그 값이 최대나 최소가 되는 상태를 해석하는 문제**
* 시스템 최적화: 시스템을 개선해서 비용을 감소시키기 위해 진행하는 행위 일체를 지칭
* Why? 고효율, 저비용
* How?: Image Analysis, Text Mining, Heuristic Algorithms

* ![image](https://user-images.githubusercontent.com/98376833/170188005-263f5d3a-f384-4a49-a564-7cf246a98b99.png)
* 위키백과: https://ko.wikipedia.org/wiki/%EC%88%98%ED%95%99%EC%A0%81_%EC%B5%9C%EC%A0%81%ED%99%94

## 시스템 (System) 정의
* 필요한 기능을 실현하기 위하여 관련 요소를 어떤 법칙에 따라 조합한 집합체
* Input 과 Output 이 존재
* ![image](https://user-images.githubusercontent.com/98376833/170244543-9d9f936d-f6a2-4589-affb-62bdb6cc1eb3.png)
* 위키백과: https://ko.wikipedia.org/wiki/%EC%8B%9C%EC%8A%A4%ED%85%9C

### 생산 시스템 예제
* Input: Material, Energy, Labor, Technology (Equipment)
* Output: Product, Scrap, Waste
* (+) 부가적인 input: Constraints, Noise

### 생산 시스템의 유형 - 생산량과 제품 다양성 측면 (The Hayes-Wheelwright Matrix)
* ![image](https://user-images.githubusercontent.com/98376833/170246582-e793495a-de3d-47f4-a46b-9c6b99950e64.png)
* https://www.productplan.com/glossary/product-process-matrix/
* 1. Job-shop
    * ex. 우주선, 항공기, 미사일, 신제품
* 2. Batch
    * ex. 가구, 책 
* 3. Mass
    * ex. 자동차, 가전제품
* 4. Continuous Flow (Process Industry)
    * ex. 가구, 책

### 생산 시스템의 유형 - 소비자의 영향 (Manufacturing Strategies)
* ![image](https://user-images.githubusercontent.com/98376833/170250806-2f4aa4bd-4299-4f12-af4e-feb89fa65ede.png)
* https://www.semanticscholar.org/paper/An-order-planning-and-scheduling-framework-in-MTO-Sun-Shi/a7435ca71806304b63b0d67a9ab3567bd96490c1
* MTS (Make-to-Stock)
* ATO (Assemble-to-Order)
* MTO (Make-to-Order)
* ETO (Engineering-to-Order)

### 생산 시스템에서 발생하는 주요 문제 
* Forecasting
* Aggregate Production Planning (APP, 총괄 생산 계획)
* Inventory Control: Deterministic Environments (결정론적 환경)
* Inventory Control: Stochastic Environments (확률적 환경)
* Supply Chain Management
* Production Control Systems: MRP and JIT
* Operations Scheduling
* Project Scheduling
* Facilities Planning
* Quality and Assurance
* Maintenance and Reliaibility

## 생산 Measure
### 생산 지표 (Plant Measures)
* Throughput: product that is high quality and is sold (단위 시간 동안 생산되는 양품의 수)
* Costs: Operating budget of plant
* Assets: Capital equipment and WIP

### 생산 목표 (Objectives)
* Maximize profit
* Minimize unit costs

### 상충 관계 (Tradeoffs)
* Increase throughput
* Decrease Cost
* Decrease Assets
