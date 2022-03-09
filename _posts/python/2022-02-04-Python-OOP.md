---
layout: post
title:  "[Python] 객체지향 프로그래밍 OOP (Object-Oriented Programming)"
excerpt: "Python 문법 1) Object란?  2) OOP (Object-Oriented Programming)"

categories:
  - python
tags:
  - [Python]

toc: true
toc_sticky: true
 
date: 2022-02-04
last_modified_at: 2022-02-04
---

## Summary
1. Object: 성질 (변수, Field)와 할 수 있는 행동 (함수, Method)를 담은 자료
2. Class: 객체를 만들 수 있는 틀
3. Instance: 클래스로 만든 객체
4. 객체지향 프로그램의 장점: 코드의 재사용 용이. 효율성 증가


## 객체 Object
### Object란?
성질 (변수) 과 할 수 있는 행동 (함수) 이 담긴 자료

### 클래스 Class
객체를 만들 수 있는 '틀' a.k.a. 붕어빵틀, blueprint

### 필드 Field
객체가 가지고 있는 성질(변수) 

### 메서드 Method
객체가 할 수 있는 행동(함수) 

-. self: self는 객체 자신을 의미함. Method라면 가져야 하는 first parmaeter (매개변수). 메서드 임을 나타내주는 규약. 메서드가 호출될 때 self 자리에 객체 자신을 인자에 넣음. 

### 인스턴스 Instance
객체를 만들 수 있는 틀(클래스) 로 찍어낸 '객체' a.k.a. 붕어빵 (instance1 = 팥붕, instance2 = 슈붕)

-. ref: 팥붕, 슈붕 처럼 인스턴스 마다 특징이 조금은 달라질 수 있음. (그래도 붕어빵인 점은 달라지지 않는다!)

ex.
```python
# 클래스 선언
class 클래스이름:
  # field
  name = "Bob"
  age = 10
  
  # method
  def exercise(self):
    print("golf")


# Instance 생성
bobby = Human()

# field of instance
bobby.name # Bob
bobby.age # 10

# method of instance
bobby.exercise() # golf
```


## 객체 지향 프로그래밍 OOP (Object-Oriented Programming)
### OOP란?
OOP는 컴퓨터 프로그래밍의 패러다임 중 하나.
컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위 ("객체") 들의 모임으로 파악하고자 함. 

### 왜 객체 지향 프로그래밍을 사용하는가?
1). 상속 Inheritance, 2). 다형성 Polymorphism, 3). 캡슐화 Encapsulation 를 통해 '코드의 재사용'이 용이하여 효율적인 코드 작성이 가능.

기존의 코딩스타일로는 표현할 수 없었던 상황욜 표현 가능하게 함. 

-. ref) Python은 기본적으로 객체 단위로 정보를 관리함. 기본적인 객체 안엔 기본적으로 값 Value, 유형 Type 속성을 가지고 있음
```python
# 클래스 선언
class 포켓몬:
  # field
  p_name = ""
  p_hp = 0
  p_type = ""
  
  # method
  def skill(self):
    pass # 아무런 동작도 하지 않는 코드. 에러 발생 방지. 
    
class 피카추(포켓몬): # 포켓몬 클래스를 상속받음
  # field
  p_name = "pika"
  p_hp = 10
  p_type = "electric"
  
  # method
  def skill(self):
    print("찌리릿")
    

# 피카추 인스턴스 생성
pika1 = 피카추()
pika1.skill() # "찌리릿"
print(pika1.p_hp) # 10

# 파이썬은 기본적으로 객체 단위로 정보 관리
base1= 2022
print(base1) #2022; Value
print(type(base1) #<class 'int>; Type
```

### 상속 Inheritance
한 클래스의 내용을 다른 클래스가 이어받는 것. 부모와 자식처럼 코드 관리 가능. 
```python
class Car:
  type = "car"
  
class BMW(Car):
  pass
  
series_4 = BMW()
print(series_4.type) #car
```

### 다형성 Polymorphism
같은 모양의 코드가 다른 역할을 하는 것. 형태가 같은데 다른 기능을 함 (다른 결과물이 나옴). 유사한 역할을 하는 변수와 메서드를 같은 이름으로 관리할 수 있음.
```python
class Car:
  type = "car"
  hp = 100
  
class Benz(Car):
  hp = 500
  
s_class = Benz()
print(s_class.type) #car
print(s_class.hp) #500
```

### 캡슐화 Encapsulation
하나의 객체를 만들 때, 그 객체가 '특정한' 목적을 잘 수행할 수 있도록 필요한 변수나 메서드를 관련성 있게 클래스에 구성
