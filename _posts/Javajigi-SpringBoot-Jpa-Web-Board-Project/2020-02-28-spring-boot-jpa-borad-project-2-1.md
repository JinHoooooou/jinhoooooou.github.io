---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(6)"
excerpt: "mustache를 활용한 동적인 HTML과 MVC 설명"

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

### 2-1. mustache를 활용한 동적인 HTML과 MVC 설명

1. Controller생성

   Controller 패키지 생성

   Controller 패키지 내 WelcomeController 클래스 생성

   mustache 템플릿 엔진을 호출하기위해서 controller가 필요

   controller 클래스 내에 mustache 템플릿 호출을 위한 메소드 생성

   template디렉토리에 있는 html파일 호출

   규칙 : 메소드가 return하는 String에 대응하는 템플릿 디렉토리 내의 파일

   --> 즉 return String이 "welcome"이라면 template디렉토리의 template.html을 호출한다.

   url 매핑을 @GetMapping으로 한다. (RequestMapping 후 Method를 get으로해도될듯)

2. mustache 템플릿 엔진에 데이터 전달

3. 동적으로 html페이지 생성

   > 404 Not Found가나온다면  .properties 파일에서 suffix를 .html으로 적용해야함

----

Spring boot Junit 5 적용

* @ExtendWith는 JUnit5 에서 반복적으로 실행되는 클래스나 메서드에 선언한다. SpringExtension는 스프링 5에 추가된 JUnit 5의 주피터(Jupiter) 모델에서 스프링 테스트컨텍스트(TestContext)를 사용할 수 있도록 해준다.

* MockMvc - 브라우저에서 요청과 응답을 의미하는 객체로서 Controller 테스트를 용이하개 해줌

* 하다가 포기.. 모르겠다 ㅜㅜ 책참고해서 별개로공부해야할듯..