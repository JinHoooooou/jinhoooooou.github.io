---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(31)"
excerpt: "QuestionController 중복 코드 제거 리팩토링"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-30
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 5-6 QuestionController 중복 코드 제거 리팩토링

* QuestionController에 보이는 여러 중복코드를 리팩토링 하는것을 먼저하고 원격 서버에 배포하자.
* 현재 로그인 되었는지 체크하는부분, question 글을 쓴 사용자와 로그인한 사용자가 일치하는지 체크하는부분이 중복이 가장많다.

* Exception을 사용하여 중복을 제거해보자

  ```java
  public class QuestionController {
    ...
    @GetMapping("/{id}/form")
    public String updateForm(@PathVariable Long id, Model model, HttpSession session) {    
      try {
        Question question = questionRepository.findOne(id);
        hasPermission(session, question);
        model.addAttribute("question", question);
        return "/qna/updateForm";
      } catch (IllegalStateException e) {
        model.addAttribute("erroMessage", e.getMessage());
        return "/users/login";
      }
    }
    private boolean hasPermission(HttpSession session, Question question) {
        if ( )
      if (!HttpSessionUtils.isLoginUser(session)) {
        throw new IllegalStationException("로그인 하세요");
      }
      
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      if (!question.isSameWriter(loginUser)) {
        throw new IllegalStationException("본인 글만 수정, 삭제할 수 있습니다.");
      }
       
      return true;
    }
    ...
  }
  ```

  * 데이터에 대해서 수정할 권한을 가지고 있는지 확인한다.
  * 다른 인증관련 로직이 있는 부분도 위처럼 수정한다.

* login.html에 로그인 관련 catch문의 model에 담은 errorMessage를 출력한다.

  ```html
  <!-- login.html -->
  ...
  <div class="panel panel-default content-main">
      {{#errorMessage}}
      <div class="alert alert-danger" role="alert">
          {{this}}
      </div>
      {{/errorMessage}}
  </div>
  ```

* 테스트

* hasPermission()이라는 메서드를 만들긴했는데 맘에들지않는다.  그래서 다른 방법을 해보려고한다.

* Validation 체크 후 Result라는 클래스를 생성에 클래스를 통해 리턴하는것이다.

  ```java
  public class Result {
    private boolean valid;
    private String errorMessage;
      
    private Result(boolean valid, String errorMessage) {
      this.valid=valid;
      this.errorMessage = errorMessage;
    }
      
    public boolean isValid() {
      return valid;
    }
    
    public static Result ok() {
      return new Result(true, null);
    }
     
    public static Result fail(String errorMessage) {
      return new Result(false, errorMessage);
    }
    
  }
  ```

* QuestionController에서 적용한다.

  ```java
  public class QuestionController {
    ...
    @GetMapping("/{id}/form") 
    public String updateForm(@PathVariable Long id, Model model, HttpSession session) {
      Question question = questionRepository.findOne(id);
      Result result = valid(session, question);
      if (!result.isValud()) {
        model.addAttribute("errorMessage", result.getErrorMessage());
        return "/user/login";
      }
      model.addAttribute("question", question);
      return "/qna/updateForm";
    }
      
    private Result valid(HttpSession session, Question question) {
      if (!HttpSessionUtils.isLoginUser(session)) {
        return Result.fail("로그인이 필요합니다.");
      }
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      if (!question.isSameWriter(loginUser)) {
        return Result.fail("본인 글만 수정, 삭제가 가능합니다.");
      }
      return Result.ok();
    }
  }
  ```

* 중복코드는 어디서나 생길 수 있다. 중복코드가 생길때마다 어떻게하면 제거 할 수 있을까 고민하는것이 개발자의 숙명이다. 그래야 코드를 더 깔끔하게 유지할 수 있다. 또 새 기능이 추가되거나 변경되었을때 빠르게 대처할 수 있다.