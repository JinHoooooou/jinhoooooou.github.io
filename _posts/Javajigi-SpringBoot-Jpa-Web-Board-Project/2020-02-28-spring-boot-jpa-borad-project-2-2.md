---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(7)"
excerpt: "회원가입 기능 구현"

categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-02-28
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 2-2. 회원가입 기능구현

1. 사용자가 입력한 데이터를 서버에서 받아서 controller에 전송

* 회원가입 페이지에서 데이터를 입력받고 제출을 누름
* 입력받은 데이터를 Controller로 보내줌
* input 태그에서 name이라는 속성을 추가해주어야 name값을 controller로 보내줌
* GET방식을 사용하면 보안상의 문제가 발생할수있음(password노출 등) --> 따라서 POST방식 사용
  * html form태그에 method=post속성추가, getMapping 대신 PostMapping 사용
* 파라미터가 많아질 수 있으니 클래스로 추출
* 클래스로 추출했을때 필드이름이 아니라 setter 메소드의 이름이 같아야 매핑이된다고함

