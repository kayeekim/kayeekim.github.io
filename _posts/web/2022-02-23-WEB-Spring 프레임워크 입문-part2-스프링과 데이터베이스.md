---
layout: post
title:  "[WEB] Spring 프레임워크 입문 Part2 - 스프링과 데이터베이스"
excerpt: "Elice강의 - Spring 프레임워크 입문- Part 2 정리"

categories:
  - web
tags:
  - [WEB]

toc: true
toc_sticky: true
 
date: 2022-02-23
last_modified_at: 2022-02-25
---

* 해당 블로그는 Elice에서 제공하는 'Spring 프레임워크 입문' 강의를 듣고 개인적으로 정리한 글입니다.

#### 전체 과정
Part 1 - 스프링 프레임워크란?
**Part 2 - 스프링과 데이터베이스**
Part 3 - 스프링과 보안
Part 4 - RESTFul API

# Part 2 - 스프링과 데이터베이스
## Summary
1) Maven 에 대해 이해하고, 라이브러리를 변경

2) Mybatis 에 대해 이해하고, 데이터베이스 연결

3) Annotation 에 대해 이해하고 활용

## Ch 1. Maven
### Maven 이란?
-. Apache 프로젝트 중 하나

-. 조금 더 편리한 '프로젝트 관리 툴'

-. 소스 코드로부터 배포 가능한 산출물 artifact 를 쉽게 build 하는 build tool

-. 자바 코드를 compile -> package -> deploy 하는 일을 '자동화'해준다.

-. Spring은 이 maven 을 활용하고 있음

### Maven 의 특징
-. 모든 것은 Maven 이 관리

-. 필요한 라이브러리 관리를 알아서 해줌 (재다운로드, 최신 버전 설치 등)

-. 특징 1) '패키징 관리'를 해준다. 사용자는 pom.xml 이라는 설정 파일에 필요한 라이브러리 & 버전 만 작성해주면 된다. (그 후 라이브러리 관리는 다 maven 이 해준다). pom.xml은 스프링 프로젝트 당 단 1개만 존재한다.

* pom.xml : Project Object Model (pom) 의 약자로 프로젝트 설정을 담당하는 파일이다. 프로젝트 정보, 라이브러리 정보, 빌드 정보 등을 관리하고 있다.

-. 특징 2) '라이프사이클' 기능을 제공한다. 

* 라이프사이클이란? maven 에 미리 정해져 있는 빌드 순서
* 1. clean: 빌드 시 생성되었던 산출물을 지운다
* 2. build: package, deploy, test 를 수행해준다
* 3. site: 문서작성 - 프로젝트 문서 작성 및 사이트 작성을 수행해준다.

-. 특징 3) '프로필 관리'를 해준다: (다른 운영체제, 다른 배포 환경, 다른 설정 체제 등) 다른 환경을 다루기 위해 프로필 관리를 해준다. 


## Ch 2. Mybatis 
### Mybatis란?
-. 자바의 관계형 데이터 베이스 (RDB) 프로그래밍을 쉽게 도와주는 "프레임워크"

-. JDBC를 보다 편리하게 사용할 수 있게 도와줌

-. JDBC는 필요할 때마다 'db 연결/ 쿼리작성/ 실행' 을 해줘야 한다. mybatis 프레임워크를 사용하게 되면 db에 접근할 때마다 중복되었던 코드 작성을 줄일 수 있다.

### Mybatis 사용 (설정) 방법
* ( 0. maven 에 mybatis 등록 - dependency 로 등록)
* 1) 별도의 xml 설정 파일에 서버 연결 정보 등록
* 2) Mapper 라는 곳에 쿼리 함수들 작성
* 3) 필요할 때마다 해당 함수 호출해 DB 연결

### Mybatis 장점 vs 단점
#### 장점
* 1. 가독성이 좋음. 코드 수정이나 인수인계가 편리하다.
* 2. sql 문이 독립되어 유지보수가 편리하다.
* 3. db 와 자바 개발이 독립이다. => db 와 자바 개발을 분리해서 작성할 수 있다.
* 4. 그렇기 때문에 db 연결 및 개발 관련 코드가 현저히 줄어든다.

#### 단점
* 1. 초기 설정이 어렵다. - xml 파일에 설정을 위해 작성해야할 정보가 엄청 많다.
* 2. 단순한 작업 (db 를 한두번만 왔다갔다 해도 될 경우) 엔 오히려 불편하다.
* 3. 설정 파일이 변경 되었을 땐, 반드시 (웹) 서버를 재시작 해야한다.


## Ch 3. Annotation
### Annotation 이란?
-. '주석' 이라는 뜻

-. 스프링에서의 어노테이션은, 클래스/메서드 같은 프로그램의 요소에 다양한 종류의 '정보를 전달' 할 수 있는 방법으로 쓰인다.

-. '@' 붙여서 사용

-. => 코드 감소, 유지보수 ('@' 만 찾으면 되므로) , 생산성이 증가한다.

### 자주 사용되는 annotation
#### @RequestMapping: URL 과 컨트롤러의 메서드를 매핑 시켜줌
```java
// @RequestMapping 을 통해, URL주소/hello 로 접근 했을 때, 자바의 hello 메서드 (public String hello()) 와 매핑

@RequestMapping(value = "/hello")
public String hello(
  System.out.println("Hello, World!");
  return "hello";
}
```

#### @RequestParam: HTTP 파라미터를 메서드 파라미터로 사용할수 있음
```java
// @RequestParam을 통해, URL주소/hello 로 접근 했을 때, '/hello?id=값' 으로 파라미터 전달 가능 (java 입장에선 값을 받아올 수 있음)

@RequestMapping(value = "/hello")
public String hello(@RequestParam int id){ // '/hello?id=값' 전달 받은 id 값을 함수인자로 사용
  System.out.println("id", id);
  return "hello";
}
```

#### @Controller: 해당 클래스가 'spring controller' 인 것을 알려줌

#### @PathVariale: URL 일부를 메서드 파라미터로 사용할 수 있음

#### @Inject: 해당 클래스에 필요한 객체를 주입 시켜 사용할 수 있게 해줌

#### @Service: 해당 클래스가 '비지니스 로직을 처리' 하는 클래스 임을 알려줌

#### @Repository: 해당 클래스가 '데이터베이스에 접근하는 함수들을 가지고' 있는 클래스 임을 알려줌 
-. Mapper 클래스 = Repository 클래스를 의미

#### @PreAuthorize: 해당 메서드의 접근 권한을 설정할 수 있게 해줌
