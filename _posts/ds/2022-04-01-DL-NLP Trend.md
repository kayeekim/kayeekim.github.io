---
layout: post
title:  "[DL] 자연어처리 기술 동향 "
excerpt: "Deep Learning - NLP - 자연어처리 기술의 다양한 활용"

categories:
  - ds
tags:
  - [DL] 

toc: true
toc_sticky: true
 
date: 2022-04-01
last_modified_at: 2022-04-13

---

# 자연어처리 기술 동향
## 자연어처리 NLP (Natural Language Processing) 
* **NLP 는 의미가 비슷한 단어끼리 수학적 공간에 서로 가까이 배치하는 작업을 수행** 한다.
* 문장/단어 의 유사도를 비교함으로서 문법 구조는 같지만 의미를 다르게 하는 작업도 가능

### 자연어 처리 동향 (기계 번역 Machine Translation 의 발전과정)
* '1950s: 기계 번역 역사 시작. 언어의 문법 규칙에 기반하는 rule-based 중심의 발전 (RBMT)
* '19990s: 통게적 확률에 기반한 모델 (SMT, Statistical Machine Translation)
* '2014~: 딥려닝 기반 방법론 (NMT). 

#### 딥러닝 기반 기계 번역 모델의 발전
* RNN -> CNN-> Transformer 기반으로 발전중

##### (2014, 2016~) RNN (with Attention) 기반
* 문장내 단어를 순차적으로 입력받아 처리
* 최초 NMT (2014): 입력문장을 처리하는 encoder, 출력 문장을 처리하는 decoder 를 모두 RNN 으로 구성
* GNMT (2016, google): Residual Connection 결합한 RNN 기반 NMT
* 단점: 순차적으로 처리해야하기 때문에 학습속도가 느리고, 긴 문장의 Gradient Vanishing 문제 여전히 대두
 
##### (2017~) CNN (with Attention) 기반
* SliceNet (google)
* ConvS2S (facebook)

##### (Recently) Transformer 기반
* Transformer
* BERT
* GPT
* *참고: Workload 특성
    * Transformer 기반 모델의 주요 연산은 Matrix Multiplication (행렬곱) 이다. 
    * 행렬곱 연산은 element-wise 연산에 비해 데이터 재사용률이 높다. 따라서, 연산 시 데이터 재사용률이 높은 Compute-intensive 특성을 가진다. 
    * ==> RNN 기반에서 Transformer 기반으로 변화함에 따라 workload 특성 또한 Memory Intensive -> Computing Intensive로 변화하고 있다.
    * (+하드웨어 측면) Compute-intensive 특성의 중요도가 부각됨에 따라 병렬 연산시 고려되는 가속기의 MAC 연산 성능과 메모리 사용이 부각되고 있다. 
        * 가속기 연산 성능 측면에서 MAC 연산에 특화된 TPU 사용이 적합하다고 한다.

#### 딥러닝 NLP 모델 발전 동향: Larger Model (모델의 퀄리티가 중요), Model Compression (모델 서비스 Latency 가 중요)
* 다음 두가지 관점에서 모델 발전이 이루어 지고 있는 추세이다.
* Larger Model: Quality 향상을 위해 모델의 복잡도를 증가
    * Motivation (Why?): 정확도 향상
    * Modeling 기법: large modeling - 모델의 복잡도를 증가 시킴
* Model Compression
    * Motivation (Why?): GPU, Memory 와 같은 리소스 한계를 극복하고 서비스 시 Latency 를 개선
    * Modeling 기법: 정확도 손실을 최소화하면서 모델 경량화 (모델 압축 등)

---

# 자연어 처리 서비스
* 기계 번역: 번역 시스템에 활용
* 질문 답변: 대화 시스템
* 텍스트 분류: 스팸 분류

## 자연어 처리를 타 분야에 활용 예시
### 이미지
#### DALL-E: GPT-3 기반의 DALL-E 
* 주어진 텍스트에 맞는 이미지를 그려냄
* DALL-E 의 학습방법
    * DALL-E 는 이미지를 단지 '레이블'로 학습 하지 않고 '설명문'으로 학습
    * 이를 바탕으로, 학습하지 않은 텍스트나 상황을 접해도 이미지를 그려낼 수 있게된다.

### 바이오
#### 바이러스 변이 예측
* 바이러스의 감염을 일으키는 특성 -> '문법'
* 항체가 탐지 못할 정도의 변이 -> '문장 의미의 변화' 로 대입

---

