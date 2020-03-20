---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(10)"
excerpt: "Mustache template engine 연습"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-02-28
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 2-5. exercise-1 mustache template engine

* 정규 과정이아닌 보너스, 2강에서 배운 mustache에 대해 더 알아볼것이다.
* mustache를 익히기 위해 hello world페이지를 이용할것이다.
* 공식문서참고해서 문법들을 익히는 과정
  * 공식문서 : [https://mustache.github.io/mustache.5.html](https://mustache.github.io/mustache.5.html) 

* html이 포함된 경우 -  {{{}}} 세번해야 적용이된다.
* 반복문 {{#name}} {{데이터}} {{/name}}
* mustache문법 테스트하며 학습하면 빠르게 익힐수 있다.