---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(12)"
excerpt: "HTML 템플릿 추가, H2 데이터베잇 설치, 관리툴 확인"

categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-03-05
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 3-1. HTML 템플릿 추가, H2 데이터베잇 설치, 관리툴 확인

* [https://github.com/slipp/web-application-server](https://github.com/slipp/web-application-server)에서 프로젝트 clone하여 webapp의 자원들 복사하여 적용, 커밋
* 데이터베이스 설정 
  * 보통은 프로그램을 설치해야하지만, H2는 별도 설치가 필요없다.
  * 근데 나는 mysql이 있기때문에 둘다 해볼것이다.
  * H2 연결
    * mvnrepository에서 h2 데이터베이스 라이브러리 pom.xml에 추가
    * url : localhost:8080/h2-console
      * 안되길래 pom.xml의 dependency의 scope삭제
      * console창은 떴는데 connect가 안됨
      * `[Database "mem:my-slipp" not found, either pre-create it or allow remote database creation (not recommended in secure environments) [90149-200\]](http://localhost:8080/h2-console/login.do?jsessionid=cd6afc27e6d38131308b305246308695#) 90149/90149`
      * 구글 검색을해보니 최근 버전 경우 보안상의 이유로 웹 콘솔에서 새 데이터베이스를 작성할 수 없다고 함.
      * 그래서 이전버전으로 바꿔보기로 결정 - 강의에 나온 버전으로 맞춰서 실행하니 잘 된다.
    * jdbc url : ~/my-slipp
    * user name sa
    * password 없
    * 테이블을 만들지 않았기 때문에 아무데이터도없다

  * Mysql 연결
    * 아직 데이터자체가 없기때문에 H2로 테스트해보고 mysql로 연동하는것 따로 포스팅할 예정

