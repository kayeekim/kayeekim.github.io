---
layout: post
title:  "[WEB] Spring 프레임워크 입문 Part4 - RESTFul API"
excerpt: "Elice강의 - Spring 프레임워크 입문- Part 4 정리"

categories:
  - Blog
tags:
  - [WEB]

toc: true
toc_sticky: true
 
date: 2022-02-25
last_modified_at: 2022-02-25
---

* 해당 블로그는 Elice에서 제공하는 'Spring 프레임워크 입문' 강의를 듣고 개인적으로 정리한 글입니다.

#### 전체 과정
Part 1 - 스프링 프레임워크란?
Part 2 - 스프링과 데이터베이스
Part 3 - 스프링과 보안
**Part 4 - RESTFul API**

# Part 4 - RESTFul API
## Summary
1) API 개념 이해

2) REST 개념 이해

3) RESTFul API 서버 생성

4) 참고: REST API 서버 생성 시 참고할 내용
* RESTFul API란? http URL 을 통해 사용자가 필요한 자원을 요청하고, 서버가 해당 자원을 http method (CRUD) 를 활용해 처리하는 것을 말함.
* spring 에서 API 서버 구현을 위한 작업: 1) pom.xml 에 필요한 외부 라이브러리 작성 / 2) RestController 구성 / 3) Domain 객체와 정보를 처리할 데이터베이스 연결작업 수행
* REST API 사용 시, @RestController 어노테이션을 사용함으로써 기존 controller와는 다르게 함수의 return이 view (jsp 파일 연결) 이 아닌, json/xml 로 리턴 (MVC 패턴에 의한 구조를 그대로 따르지 않음)

## Ch 4-1. RESTFul API
### API 란?
-. 프로그램과 프로그램을 연결 해주는 다리의 역할

-. 예시) 네비게이션 앱을 만들 때 지도 데이터는 '네이버/카카오 지도 API 활용'이 가능하다.

### RESTFul API 란?
-. REST (REpresentational State Transfer) 

-. 사전적 의미: 자원을 이름 (자원의 '표현 represent') 으로 구분하여, 해당 자원의 정보 (상태; state) 를 주고받는 모든 것

-. 데이터가 요청되는 시점에서의 정보 전달. 주로 json 혹은 xml 파일 활용

-. HTTP URI 를 통해 해당 자원을 명시하고. HTTP Method (CRUD 크루드) 를 통해 자원을 처리.

-. CRUD (- 사용하는 함수)
* C: Create 생성 - POST 
* R: Read 조회 - GET
* U: Update 수정 - PUT
* D: Delete 삭제 - DELETE

-. 클라이언트 -> (url 로 요청) -> 서버 

-. 서버 -> (json, xml 로 응답) -> 클라이언트

### URL 파라미터
-. RESTFul API 를 활용하기 위해 URL 에 파라미터를 함께 보내는 방법 (정보를 실어서 보내는 방법)

* 첫 번째 파라미터: ? 활용
* 두 번째 부터 : & 활용
* ex) https: test.com?id=아이디&pw=비밀번호&..

### RESTFul API 예시
```
# 클라이언트에 응답할 json 파일 예시
{
  "id": 1,
  "firstName": "Framework",
  "lastName": "Spring",
  "classes": [ {"id": 1,
                "name": "JAVA"},
                {"id": 2,
                "name": "HTML"},
                {"id": 3,
                "name": "Mysql"} ]
 }
 ```
 
 -. (URI: students 로 작업 예제) CRUD 메서드를 통해 아래 내용 수행 가능
 * [POST] /students: 새 student 를 등록
 * [GET] /students: 전체 student list 호출
 * [GET] /students/1: 1번 student 호출
 * [PUT] /students/1: 1번 student 의 정보 수정
 * [DELETE] /students/1: 1번 student의 정보 삭제

### 일반 API vs RESTFul API
-. RESTFul API 는 정해진 규칙대로 주소를 만드므로, 1) "일관성"이 있고 2) "확장성"이 좋다.

|기능|일반 API|RESTFul API|
|------|---|---|
|새로운 학생 생성|/CreateStudent|[POST]/student|
|이름으로 학생 검색|/getStudentByName|[GET]/student?name=이름|

### http 응답 코드
-. http 응답 코드로 응답 상황 확인 가능

* 1xx (1로 시작): 정보를 전달했고, 요청을 받았으며 작업 진행 중이다.
* 2xx: 클라이언트 요청이 성공적으로 수행되었다. 
* 3xx: 요청을 완료 했으나, 요청을 완벽하게 수행하기 위한 re-direction (다른 페이지로 이동) 이 필요하다.
* 4xx: 오류 - 클라이언트 (사용자) 의 잘못된 요청 ex. 404에러
* 5xx: 오류 - 서버쪽의 오류 ex.5 00에러


## Ch 4-2. REST API 서버 실습
-. REST API 를 사용하기 위해서 아래 사전작업이 필요하다.

### 1) API 로 제공할 resource 정보 구축
-. 필요한 수집 데이터

### 2) Domain 객체 생성
-. 리소스 정보를 담이서 json/xml로 전달할 클래스를 생성해주어야 한다.

### 3) repository 객체 생성
-. Mybatis 기반 mapper 인터페이스 구성필요

* 값을 입력/조회/수정/삭제 할 CRUD 방식으로 작동할 때 mapper 필요
* 데이터 베이스 연결 정보와 관련된 쿼리함수들

### 4) RESTController 구성
* @RestController 어노테이션: 컨트롤러 어노테이션과 비슷하지만, 응답을 jsp 파일로 보내지 않는다는 차이점이 있다. * REST API 에서는 응답을 json/xml 파일로 보낼 것이므로
* @Inject 로 Mapper 주입: 데이터베이스와 소통하기 위해, inject 어노테이션으로 mapper 를 의존성 주입
* @RequestMapping 로 URL 설정: RequestMapping 어노테이션을 통해, 각각의 REST API 에 URL 설정 가능

### 5) GET 서비스
-. read "요청"을 받으면 json 형태로 리턴

### 6) POST 서비스
-. 새로운 resource 정보를 서버의 자원으로 "생성"

### 7) RESTFul API 테스트 (방법)
-. Controller 의 @RequestMapping 정보를 통해 접근 url 확인 및 요청

-. ex. 접근 url: http://localhost:8080/temp/temperature/23 (+) 요청방식: GET 방식 일 경우 => 테스트 내용  23번 온도정보를 출력해달라

