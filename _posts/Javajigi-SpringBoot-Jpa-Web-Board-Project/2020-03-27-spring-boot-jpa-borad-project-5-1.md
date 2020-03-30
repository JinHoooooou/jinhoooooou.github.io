---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(28)"
excerpt: "회원과 질문간의 관계 매핑 및 생성일 추가"
classes: wide
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

### 5-1 회원과 질문간의 관계 매핑 및 생성일 추가

* 데이터베이스에서 one-to-many, many-to-one, many-to-many개념이 있는데, 회원과 질문간에도 같은 방식으로 매핑할 수 있다.

  * 1명의 회원은 여러개의 질문을 남길 수 있다.
  * 각각의 질문은 한명의 회원이 쓸 수 있다.
  * 회원 - 질문 : one - to - many / 질문 - 회원 : many - to - one

* 관계를 맺을 때 User에서 Question에대한 목록을 가지고 있도록관계를 맺을 수 있고, Question에 String으로 되어있는 writer를 User객체로 바꾸어 관게를 맺을 수 있다. 쌍방의 관계를 맺도록 설계할 수도 있다.

  * 대부분의 객체설계에서 회원 데이터가 가장 최상위에 놓인다.

* Question에서 User를 볼 수 있도록 관계를 맺어 보자.

  ```java
  @Entity
  public class Question {
    @Id
    private Long id;
    @ManyToOne
    @JoinColumn(foreignKey =  @ForeignKey(name = "fk_question_writer"))
    private User writer;
    ...
    public Question(User writer, String title, String contents) {
      this.writer = writer;
      this.title = title;
      this.contents = contents;
    }
  }
  ```

  * `String writer`대신 `User writer`사용
    * Constructor나 QuestionController.java도 변경된 사항 수정
    * 객체에서 getMethod를 사용하여 객체를 꺼내오려는 작업이 많을수록 리팩토링 요소이다. 객체를 꺼내오는것보다 객체를 전달하는것이 좋다.
  * Question과 User관계는 Many-to-one이므로 어노테이션 추가
  * `@JoinColumn(foreignKey = @ForeignKey(name = "fk_question_writer"))`로 foreign key의 이름을 지정할 수 있다.
  * 테스트를 해보면 Question 테이블에 Writer_ID가 나오는것을 볼 수 있다. Foreign Key로 지정했기 때문에 User객체의 ID인 `Long id`가 출력되는것이다.

> 여기까지 실습

* 테스트 결과 출력화면에서 writer가 `User.toString()`로 나오기 때문에 수정해야한다. index.html에서

  ```html
  <!-- index.html-->
  ...
  <a href="./user/profile.html class="author">{{writer.userId}}</a>
  ```

* 날짜 시간이 고정되어있다. 이 부분도 수정해보자.

  ```java
  //Question.java
  @Entity
  public class Question {
    ...
    private LocalDateTime createDate;
    ...
    public Question(User writer, String title, String contents ) {
      this.writer = writer;
      this.title = title;
      this.contents = contents;
      this.createDate = LocalDateTime.now();
    }
    
    private String getFormattedCreatedDate() {
        return createDate.format(DateTimeFormatter.ofPattern("yyyy.MM.dd HH:mm:ss"));
    }
  }
  ```

  * [LocalDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html) 클래스는 자바8에 추가된 클래스이다. 기존에 날짜관련 자바 API가 좋지 않아서 외부 API를 쓰곤 했는데 자바8에 추가되어 그냥 쓰면된다.
  * `LocalDateTime.now()`를 DB로 확인해보면 이상한 문자열이 출력되는데 이를 포맷팅하기 위해 `getFormattedCreatedDate()`메서드를 추가했다.

* index.html에서도 시간 출력부분을 수정한다.

  ```html
  <!-- index.html -->
  ...
  <span class="time">{{formattedCreatedDate}}</span>
  ```

  * jsp에서는 get메서드의 get을 지우고 메서드명을 적어주면 된다고 해서 테스트 해봤는데 mustache도 똑같이 적용된다.

> 여기까지 실습

* :raising_hand_man: 실습 할 때 createDate를 LocalDateTime객체로 선언하지않았다. 어짜피 now메서드가 static이므로 `LocalDateTime.now().format(DateTimeFormatter.ofPattern())`으로 inline으로 구현했다.

  ```java
  private String createDate;
  
  public Question(User writer, String writer, String contents) {
    super();
    this.writer = writer;
    this.title = title;
    this.contents = contents;
    this.createDate = LocalDateTime.now()
        .format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
  }
  ```
