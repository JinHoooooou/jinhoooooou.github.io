---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(30)"
excerpt: "수정/삭제 기능에 대한 보안 처리 및 LocalDateTime 설정"
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

### 5-4 수정/삭제 기능에 대한 보안 처리 및 LocalDateTime 설정

* 질문 수정/삭제에 대한 보안처리를 하여 좀 더 안전하게 서비스하도록 구현해보자.

* 기본적인 CRUD기능을 구현하긴 했는데 치명적인 문제가있다.

  * 수정을하고 삭제를 하는데 내가 쓴 글인지 체크하는 부분이 없기 때문에 나 말고 다른 사용자가 글을 수정하거나 삭제할 수 있다.
  * 백엔드 개발을 할때 기능적인 구현 + 보안처리를 항상 생각해야한다.

* updateForm page로 갈 때 로그인을 했는지, 로그인 한 사용자가 글 쓴 사용자와 일치하는지 체크하는 로직이 필요하다.

  ```java
  public class QuestionController {
    ...
    @GetMapping("{id}/form")
    public String updateForm(@PathVariable Long id, Model model, HtppSession session) {
      if (!HttpSessionUtils.isLoginUser(session)) {
        return "/users/loginForm";
      }
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      Question question = questionRepository.findOne(id);
      if(!question.isSameWriter(sessionUser)) {
        //오류메시지
      } 
      ...
    }
  }
  ```

  * createQuestion메소드에서 로그인 체크 로직을 그대로 가져왔다.

  * 로그인한 user정보와 question을 작성한 user정보가 일치하는지 확인하기 위해서 메시지를 객체로 넘긴다(get메서드를 사용하는것은 좋지 않기 때문에)

    ```java
    public class Question {
      ...
      public boolean isSameWriter(User loginUser) {
        return this.writer.equals(loginUser);
      }
    }
    ```

    * 이 메서드는 항상 false를 리턴한다. equals메서드는 인스턴스가 달라도 각 인스턴스가 가지고 있는 값이 같을경우 true로 반환하기 때문이다.
    * User class에서 equals()와 hashCode()를 override를 해줘야한다.
    * :raising_hand_man: hasCode, equals 오버라이드를 왜 하는건지 잘 와닿지 않아서. 나는 writer.equals를 사용하지않고 User객체에 isSameWriter메소드를 만들어서 `this.id.equals(loginUser.id)`로 구현했다.

* 같은 방식으로 updateQuestion 메서드와 deleteQuestion메서드도 로직을 추가해준다.

  ```java
  public class QuestionController {
    ...
    @PutMapping("{id}")
    public String update(@PathVariable Long id, String title, String contents, HttpSession session) {
      if (!HttpSessionUtils.isLoginUser(session)) {
        return "/users/loginForm";
      }
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      Question question = questionRepository.findOne(id);
      if(!question.isSameWriter(sessionUser)) {
        //오류메시지
      }
      ...
    }
      
    @DeleteMapping("{id}")
    public String delete(@PathVariable Long id, HttpSession session) {
      if (!HttpSessionUtils.isLoginUser(session)) {
        return "/users/loginForm";
      }
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      Question question = questionRepository.findOne(id);
      if(!question.isSameWriter(sessionUser)) {
        //오류메시지
      }
      ...
    }
  }
  ```

* 테스트

> 여기까지 실습

* H2 콘솔의 Question 테이블을 확인해보면 create_date의 타입이 바이너리이다. 그래서 이것을 timestamp로 수정하려고한다.
  
  * 그런데 확인해보니 나는 타입이 timestamp이다. 굳이 따라 고칠필요를 못느껴서 따라하지않음
  
  * :raising_hand_man:그런데 나는 5-1에서 createDate 필드를 String으로 선언해서 데이터베이스에서 확인해보면 타입이 varchar이었다. 그래서 다시 LocalDateTime객체로 구현했다.
  
    ```java
    public class Question {
      ...
      private LocalDateTime createDate;
      ..
      public Question(User writer, String title, String contents) {
        super();
        ...
        this.createDate = LocalDateTime.now();
      }
    }
    ```
  
    

