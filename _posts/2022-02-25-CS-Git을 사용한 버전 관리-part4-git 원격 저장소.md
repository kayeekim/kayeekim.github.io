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

-. "-v" 옵션을 사용하면 지정한 원격 저장소의 이름과 주소를 함께 확인 가능: ``` git remote -v ```

* 원격 저장소 이름 변경: ``` git remote rename {(구)저장소명} {(신)저장소명} ```
* 원격 저장소 삭제: ``` git remote rm {삭제할 저장소명}```
* 원격 저장소 갱신: ``` git pull {동기화할 원격저장소명} {갱신할 브랜치명} ``` (자동으로 로컬저장소와 merge 까지 수행) , ``` git fetch ```
* 원격 저장소 발행: ``` git push {동기화할 원격저장소명} {발행할 정보가 있는 브랜치명} ```

#### [Remind] git diff
-. ``` git diff ``` # commit 된 파일 상태와 현재 수정중인 상태 비교

-. ``` git diff {비교할 branch1} {비교할 branch 2} ``` # branch 간의 상태 비교 ex. git diff feature/test origin/master

## Ch 1. 원격 저장소 받아오기 
-. 원격 저장소란? 인터넷이나 네트워크 어딘가에 있는 저장소 (호스팅 서비스)

### git 원격 저장소 받아오기
-. 기존 git repository 복사: ```git clone ``` # 원격/로컬 저장소 복사

-. url 로 받아오기: ``` git clone {HTTP 형태로 존재하는 원격 저장소 주소; Clone with HTTPS} ```

#### tip) 원격 저장소 받아올 때, 현재 폴더를 저장소로 쓰기
-. git clone 명령어를 실행 할 경우, 현재 로컬 폴더 내에 새로운 폴더를 하나 더 생성하게 됨. 만약 현재 위치한 경로 (폴더) 자체를 저장소로 쓰고 싶다면 'git clone' 명령 마지막에 '. (마침표)' 작성

### 원격 저장소 추가 (연결)
-. 원격 저장소를 쓰기 위해, 사용할 원격 저장소와 로컬 저장소를 연결

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

``` git pull {동기화할 원격저장소명} {갱신할 브랜치명} ```

-. fetch: 원격 저장소에서 데이터 가져오기 * 자동으로 병합을 수행하지 않음

#### tip) git pull 시, 충돌 방지를 위해 웹 호스팅 서비스 (e.g. github) 에서 제공하는 merge request 활용 가능
-. 하나의 브랜치에서 여러 사람이 동시에 작업할 경우, 다른 사람이 올린 commit 의 내용과 내 컴퓨터에 존재하는 내용이 충돌할 수 있음.

-. 이를 방지 하기 위해, 여러 개의 브랜치를 나누고 각자의 브랜치에서 작업한 후에 웹호스팅 서비스에서 존재하는 merge request 로 하나씩 합쳐가는 방식을 사용하면 충돌이 일어나는 것을 막을 수 있다 => 매번 새롭게 merge 해야하는 수고를 덜 수 있다.


### 저장소 발행
-. 로컬 저장소에서 작업한 내용을 원격 저장소에 반영

-. 단, 다른사람이 먼저 push 한 상태에서는 push 불가. 이 경우, 다른 사람이 작업한 것을 받아와서 (pull/fetch), merge 부터 진행.

``` git push {동기화할 원격저장소명} {발행할 정보가 있는 브랜치명} ```


## Ch 3. Origin 이란?
-. Origin이란?: 기본적으로 만들어질 원격저장소의 이름 (default 값)

-. (=>) clone으로 복사해온 저장소의 이름은 origin 으로 통일

-. "-v" 옵션을 사용하면 지정한 원격 저장소의 이름과 주소를 함께 확인 가능

``` git remote -v ```

