---
layout: post
title:  "[AI] 연합 학습 Federated Learning"
excerpt: "Federated Learning"

categories:
  - ds
tags:
  - [AI]

toc: true
toc_sticky: true
 
date: 2022-03-09
last_modified_at: 2022-03-09

---

최근 Federated Learning 이란 키워드를 자주 접하여, 궁금해서 찾아보았다.  
(특히 통신 분야에서 자주 등장하는 듯하다.) 2017년 구글 AI블로그에서 소개 되었으며, 소개 당시 Gboard on Android
에 테스트 중이라고도 밝혔다 
> We're currently testing Federated Learning in Gboard on Android, the Google Keyboard. When Gboard shows a suggested query, your phone locally stores information about the current context and whether you clicked the suggestion. Federated Learning processes that history on-device to suggest improvements to the next iteration of Gboard’s query suggestion model.

2019년에 google FL workshop 이 개최되었고 (Workshop on Federated Learning and Analytics; https://sites.google.com/view/federated-learning-2019/home), tensorflow 에서도 _TensorFlow Federated (TFF) 프레임워크_ 를 제공하고 있는 걸 보니
주목받는 키워드라고 할 수 있겠다.


## What is Federated Learning (FL)
> Standard machine learning approaches require centralizing the training data on one machine or in a datacenter. And Google has built one of the most secure and robust cloud infrastructures for processing this data to make our services better. Now for models trained from user interaction with mobile devices, we're introducing an additional approach: Federated Learning.

구글 블로그에 소개된 첫문장이다. 여기서 주목할만한 키워드는 _centralizing (Standard machine learning 특징)_ 와 _more secure_ 다.

Federated Learning 은 탈중앙화 (Decentralizing) 상황에서 모델을 학습시키는 방법이라고 이해했다. 즉, 하나의 중앙 서버; one machine or a datacenter 에 training data 를 모아
모델을 학습시키는 Standard ML과 달리 "Federated Learning 기술은 device (local client) 와 중앙 서버 (e.g. cloud)가 협력하여 모델을 학습시킨다. 
이 때, 학습할 데이터에 대한 협력이 아닌 _모델정보에 대한 협력_ 만 이뤄지기 때문에, more secure 하다고 할 수 있다. 

통신 분야에서 해당 키워드가 자주 등장하는 이유는, 이 FL 기술을 사용하여 _개인정보 유출을 방지하면서 ML 모델 학습을 수행_ 할 수 있다는 장점이 있기 때문으로 보인다.

## How to.?
> It works like this: your device downloads the current model, improves it by learning from data on your phone, and then summarizes the changes as a small focused update. Only this update to the model is sent to the cloud, using encrypted communication, where it is immediately averaged with other user updates to improve the shared model. All the training data remains on your device, and no individual updates are stored in the cloud.  
![image](https://user-images.githubusercontent.com/98376833/157426012-ed33e0c7-82f7-4847-9750-bd703ea4c78e.png)  
_Your phone personalizes the model locally, based on your usage (A). Many users' updates are aggregated (B) to form a consensus change (C) to the shared model, after which the procedure is repeated._

각각의 local device (local client) 에서 current global model 을 다운받은 후, local device 에 있는 데이터로 모델 (local model) 을 학습시킨다. 
학습시킨 모델 (local model) 은 encrypted communication 을 통해 중앙서버로 보내지고, 중앙 서버 (e.g. cloud) 에선 globalization (평균을 내는 등) 을 수행한다 (global model update) . 

여기서, FL 기술의 핵심 포인트는 중앙 서버에 데이터를 직접 업로드하는 것이 아닌 각 local device 에서 학습된 모델의 정보 (e.g. 모델 parameter) 만 업로드한다는 것이다.

## FL의 장점
> Federated Learning allows for smarter models, lower latency, and less power consumption, all while ensuring privacy.

FL의 장점으론 크게 1) Privacy 보장 2) 효율적인 커뮤니케이션 (Low Latency) 를 꼽을 수 있겠다. 

* 1) Privacy:  데이터를 직접 전송할 필요가 없다는 점에서, 데이터 유출없이 학습이 가능하고
* 2) Efficient Communication: FL 방법을 사용하면, local model 의 업데이트된 정보만 주고받으면 되므로 데이터를 중앙 서버에 전송하는데 필요로 하는 네트워크 트래픽과 storage 비용을 줄일 수 있다. 

## FL의 Challenge
구글의 블로그에도 소개되어있듯, Federated Learning 을 가능하게 하려면 많은 algorithmic and technical challenges 해결이 필요하다.

최근 FL 기술의 challenge 가 정리된 블로그를 하나 가져왔다 (_연합 학습(Federated Learning), 그리고 챌린지_ LINK. 참고)

레퍼로 참고한 블로그를 보면 크게 아래 3가지 관점으로 분류해 두었다. 해당 내용 관련해서는 추후 좀 더 공부할 예정이다.
* 탈중앙 학습 Decentralized Learning 에서 오는 문제점: 네트워크 구조/비동기 통신 문제, Local Update 방식 결정, 신뢰 문제 (공격과 데이터 신뢰성), Pesronalized model 고려 방법 등
* Non-IID 데이터가 가져오는 Improving Efficiency 문제: Non-IID (Non Independent and Identically Distributed) 데이터란 local client 가 가지고 있는 데이터들에 대해 독립을 가정할 수 없고, 동일한 확률 분포 또한 가지고 있지 않다는 의미이다. 이를 해결하기 위한 방법 중 하나로 _데이터 셋 증류 (Dataset Distillation)_ 방법이 언급되고 있다.
* ML 학습 실패 & 시스템 공격으로 인한 문제: 학습 실패 - 단순 버그, noise data, 비신뢰 환경 / 시스템 공격 - Model update poisoning, Data poisoning, Evasion attack

---

###### 참고링크
1. https://ai.googleblog.com/2017/04/federated-learning-collaborative.html  
2. 연합 학습(Federated Learning), 그리고 챌린지 LINK. https://medium.com/curg/%EC%97%B0%ED%95%A9-%ED%95%99%EC%8A%B5-federated-learning-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%B1%8C%EB%A6%B0%EC%A7%80-b5c481bd94b7
