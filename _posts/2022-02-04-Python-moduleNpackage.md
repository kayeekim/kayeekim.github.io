---
layout: post
title:  "[Python] 모듈 Module"
excerpt: "Python 문법 1) Module 이란?  2) Module vs Package"

categories:
  - Blog
tags:
  - [Python]

toc: true
toc_sticky: true
 
date: 2022-02-04
last_modified_at: 2022-02-04
---

## Summary
1) Module: 특정 목적을 가진 함수, 자료, 코드의 모임
2) 사용자 정의 모듈: .py 로 제작 가능
3) package: module을 폴더로 구분 (관리 목적)444

## 모듈 Module
### Module이란?
특정 목적을 가진 함수, 자료의 모임

### Module 사용하기
import 를 사용하여 module 을 불러올 수 있다

ex.
```python
import random

# .(dot)을 사용하여 모듈 속 함수/변수 사용
print(random.randrange(0,2)) # 0 이상 2 미만 수 중 임의의 수 출력
```

### 사용자 정의 모듈 
.py 파일로 만들어서 사용

ex.
```python
# cal.py

# 모듈 속 변수 정의
modelName = 'calculate' 

# 모듈 속 함수 정의
def plus(a,b):
  c = a + b
  return c
```

```python
# main.py
import cal

print(cal.modelName) # calculate
print(cal.plus(3,4)) # 7
```

### 모듈 활용
```python
import math # 수학연산 
import random # 임의의 수 선택

math.pi # pi값 (3.14...)
math.e # 자연상수 (2.71...)
random.randrange(a,b) # a이상 b미만의 수 중 하나 반환
```


## 패키지 Package
### package란?
모듈을 폴더 directory로 구분하여 '관리하는 것'. 모듈을 계층적으로 관리하는 것. 모듈이 모여있는 폴더를 package라고 부른다.

모듈을 편리하게 관리하기 위해 package 사용; 폴더로 관리


### Package 사용하기
1) import를 이용해서 폴더/모듈을 불러온 후 함수 실행
```python
import user.cal 

print(cal.plus(3,4))
```

2) from-import 사용: 함수/변수 사용시 .(dot) 사용할 필요 없음
``` python
from user.cal import plus # from {module/pkg} import {func}

print(plus(3,4))
```


