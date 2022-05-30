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
    * Contents based, Collaborative (Memory based : Item based, User based / Model based: Latent factor based)


