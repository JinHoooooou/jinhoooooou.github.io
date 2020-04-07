---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(22)"
excerpt: "질문하기, 질문목록 기능 구현"

categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-03-16
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 4-5.질문하기, 질문목록 기능 구현

* 글쓰기 화면으로 이동하려면 QNA form 페이지를 templates 디렉토리로 옮겨야한다(user관련 페이지들 옮긴것처럼)
  * templates 디렉토리에 qna 디렉토리생성 후 form.html 이동 후 import작업도 수행
* qna기능이 추가되었기때문에 UserController가아닌 새로운 Controller를 생성해준다.
  * 질문과 답변을 나누어 관리하기 위해 QuestionController만 추가해본다.
* 메인페이지의 질문하기 버튼의 url 수정해준다.
* 질문 form페이지를 보면 작성자쓰는 칸이 있는데 작성자 칸을 지우고 로그인했을때만 질문할 수 있도록 수정해보자
* 그리고 회원가입처럼 질문을 등록하면 메인 페이지로 redirect되도록 수정한다. 그리고 글쓰기도 역시 로그인되어있어야한다.
* 질문목록을 데이터베이스에 저장하기위해 Question 클래스를 생성하여 데이터베이스와 매핑한다.
  * writer, title, contents
  * 생성자를 만들어준다.(기본생성자, 생성자오버로딩)

* 데이터베이스의 쿼리작업을 위한 Repository Interface를 생성한다.
* 메인페이지에 작성한 질문들 리스트가 보이도록 HomeController에도 questionRepository선언을 해주고, Model을 이용하여 index.html로 등록한 question정보를 전달한다.