---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(27)"
excerpt: "다섯 번째 반복주기 학습 목표 및 과정 설명"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-27
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 5-0 다섯 번째 반복주기 학습 목표 및 과정 설명

* JPA를 사용하며 서로 객체들간의 관계를 맺는법에 대해 공부할것이다.

* Question.java를 보면 Question을 로그인한 사용자가 글을 쓰기 때문에 특정 사용자의 정보를 가지고 있어야한다.

* 객체를 설계하는것을 보면 String Writer --> 로그인한 사용자의 정보를 가지고있다. 

* 테이블을 설계할때 보통 사용자(User)객체의 Long id를 가지고 있는 형식으로 많이 구현을 하는데 자바 객체 상태에서 개발할때도 보면 그런식으로 하는 경우를 많이 본다.

  ```java
  //Question.java
  public class Qusetion {
      @Id
      private Long id;
      
      private Long writerId;
      ...
  }
  ```

* DB 테이블을 설계할 때, 위처럼 하는 경우가 많다. 그렇다 보니 자바 객체를 설계할때도 User객체의 Primary Key에 해당하는 id값만 가지는 경우가 일반적으로 많다.

* 그런데 이렇게하면 객체지향패턴으로 개발하는데 한계가 있다. 객체지향이라는것은 객체와 객체간의 협력을하며 로직처리를해야하는데 User와 Question이 따로라면 Question에서 조회한 writerID에 대한 User객체를 또 다시 데이터베이스에서 가져와서 생성 후 User와 Question 객체가 협력해야하는 형태가 되는데, 이는 상당히 불편하다.

* 좀 더 객체지향적인 개발을 하고 싶다면 Question내부에 User객체 인스턴스를 생성하는것이 좋다.

* 객체끼리 관계를 맺고 협력하는 구조가 객체지향의 핵심인데 잘 안지키는 사람들이 많다.

* 이런 객체끼리의 관계를 맺는데 JPA를 사용하면 어노테이션만 사용하면 되기 때문에 훨씬 더 쉽게 할 수 있다.

  

* 이런 객체간의 관계 매핑을 배우고

* 질문 상세보기 기능 구현

* 질문 수정 구현

* 질문과 답변간의 관계도 있으니 매핑하고

* 답변 목록 기능 구현

* 배포