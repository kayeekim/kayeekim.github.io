---
layout: post
title:  "[Git] Part4 - Git 원격 저장소"
excerpt: "Elice강의 - Git을 사용한 버전 관리- Part4 정리"

categories:
  - Blog
tags:
  - [Git]

toc: true
toc_sticky: true
 
date: 2022-02-25
last_modified_at: 2022-02-25
---

* 해당 블로그는 Elice에서 제공하는 'Git을 사용한 버전관리' 강의를 듣고 개인적으로 정리한 글입니다.

#### 전체 과정
Part 1 - Git 이란?
Part 2 - Git 시작하기
Part 3 - Git 가지 치기
**Part 4 - Git 원격 저장소**

# Part 4 - Git 원격 저장소  
## Summary
1) 로컬 저장소를 원격 저장소와 연결

2) 원격저장소에서 작업 내용을 받아옴

3) 원격 저장소에 작업 내용 반영

### 원격 저장소 관련 명렁어
* 기존 git repository 복사: ``` git clone {저장소 주소}```
* 원격 저장소 추가(연결) : ``` git remote add {지정할 원격저장소 단축이름} {원격저장소 주소} ```
* 연결된 원격 저장소 확인: ``` git remote ```, (원격 저장소 한눈에 보기) ``` git remote show origin ```

-. "-v" 옵션을 사용하면 지정한 저장소의 이름과 주소를 함께 확인 가능: ``` git remote -v ```

* 원격 저장소 이름 변경: ``` git remote rename {(구)저장소명} {(신)저장소명} ```
* 원격 저장소 삭제: ``` git remote rm {삭제할 저장소명}```
* 원격 저장소 갱신: ``` git pull ``` (자동으로 로컬저장소와 merge 까지 수행) , ``` git fetch ```
* 원격 저장소 발행: ``` git push ```

## Ch 1. 원격 저장소 받아오기 
-. 원격 저장소란? 인터넷이나 네트워크 어딘가에 있는 저장소 (호스팅 서비스)

### git 원격 저장소 받아오기
-. 기존 git repository 복사: ```git clone ``` # 원격/로컬 저장소 복사

-. url 로 받아오기: ``` git clone {HTTP 형태로 존재하는 원격 저장소 주소; Clone with HTTPS}

### 원격 저장소 추가
-. 원격 저장소 추가(연결) : ``` git remote add {지정할 원격저장소 (단축)이름} {원격저장소 주소} ``` # git remote add myproject https://github.com/...
 
-. 원격 저장소 주소 구성: 웹 호스트 서비스/그룹명/프로젝트명

ex. https://gitlab.com/group/project 
* 웹 호스트 서비스: ex.gitlab.com
* 그룹명 (작업을 진행하는 그룹명): ex. group
* 프로젝트명: ex. project

-. 연결된 원격 저장소 확인: ``` git remote ```, (원격 저장소 한눈에 보기) ``` git remote show origin ```

### 원격 저장소 이름 변경
-. 원격 저장소 이름 변경: ``` git remote rename {(구)저장소명} {(신)저장소명} ``` # ex. git remote rename origin git_test # origin -> git_test 로 이름 변경

### 원격 저장소 삭제
-. 주소가 변경되었거나, 필요 없어진 저장소는 삭제

-. 원격 저장소 삭제: ``` git remote rm {삭제할 저장소명}```


## Ch 2. 원격 저장소 동기화
### 저장소 갱신
-. pull: 원격 저장소에서 데이터 가져오기 + (자동으로) 현재 로컬 데이터와 병합 merge 

``` git pull ```

-. fetch: 원격 저장소에서 데이터 가져오기 * 자동으로 병합을 수행하지 않음

### 저장소 발행
-. 로컬 저장소에서 작업한 내용을 원격 저장소에 반영

-. 단, 다른사람이 먼저 push 한 상태에서는 push 불가. 이 경우, 다른 사람이 작업한 것을 받아와서 (pull/fetch), merge 부터 진행.

``` git push ```


## Ch 3. Origin 이란?
-. Origin이란?: 기본적으로 만들어질 원격저장소의 이름 (default 값)

-. (=>) clone으로 복사해온 저장소의 이름은 origin 으로 통일

-. "-v" 옵션을 사용하면 지정한 저장소의 이름과 주소를 함께 확인 가능

``` git remote -v ```

