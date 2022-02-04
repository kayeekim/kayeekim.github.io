---
layout: post
title:  "[Python] 함수 Function 이란? 함수 Function vs 메서드 Method, 매개변수 Parameter vs 인자 Argument"
excerpt: "Python 문법 1) Function vs Method 차이점이 무엇인지. 2) Parameter vs Argument 차이점"

categories:
  - Blog
tags:
  - [Python]

toc: true
toc_sticky: true
 
date: 2022-02-24
last_modified_at: 2022-02-24
---


## Function vs Method
### function란? 
특정 기능을 수행하는 코드(들의 모음) . 매개변수 parameter를 이용해 자료 전달. 

구현: 함수명(매개변수)

-. Ref. 내장 함수 vs 외장 함수 (+ 사용자 지정 함수), 전역 변수 vs 지역 변수의 차이

### method란? 
함수 중에서 "특정 자료와 함께" 사용되는 함수. 특정 자료와 연관지어 기능.  

구현: 자료.메서드함수명()

## Parameter vs Argument
### Parameter란 (매개변수)? 
함수를 정의할 때의 input.
### Argument란 (인자)? 
함수를 호출할 때 (이미 정의된 함수를 사용할 때) 의 input.

ex. 

```python
    def plus(a,b): ... # 이 때 a,b는 parameter
    print(plus(a,b)) # 이 때 a,b는 argument
```