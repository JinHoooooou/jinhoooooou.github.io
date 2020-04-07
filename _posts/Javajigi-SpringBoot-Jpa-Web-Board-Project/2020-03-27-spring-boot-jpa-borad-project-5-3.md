---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(30)"
excerpt: "질문 수정, 삭제 기능 구현"
classes: wide
categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-03-27
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 5-3 질문 수정, 삭제 기능 구현

* 질문 읽는 페이지를 보면 수정, 삭제, 목록 link가 있는데 이것을 구현해보자.

* show.html에서 수정, 삭제, 목록 link를 바꾸자

  ```html
  <!-- show.html -->
  ...
  <a class="link-modify-article" href="/questions/{{id}}/form">수정</a>
  ...
  <a class="link-modify-article" htef="/">목록</a>
  ```

* 수정에 대한 link를 만들었으니 뭘 만들어야할까? 당연히 Controller의 메서드이다.

  ```java
  public class QuestionController {
    ...
    @GetMapping("{id}/form")
    public String updateForm(@PathVariable Long id, Model model) {
      model.addAttribute("question", questionRepository.findOne(id));
      return "qna/updateForm";
    }
  }
  ```

  * 전에 생성한 question 데이터를 updateForm으로 전달해야하므로 Model을 사용한다.

* updateForm에 대한 html 파일도 qna 디렉토리에 생성한다.

  ```html
  <!-- updateForm -->
  <div class="panel panel-default content-main">
    {[#question]}
      <form name="question" method="post" action="/questions/{{id}}">
        <input type="hidden" name="_method" value="put" />
          ...
          <... value="{{title}}" />
          <... value="{{contents}}" />
      </form>
    {{/question}}
  </div>
  ```

* 테스트

  * 질문 데이터도 계속 서버를 재시작할때마다 초기화되기때문에 query문을 미리 만들어두면 좋을것같다.

* updateForm.html에서 form을 submit하기 때문에 수정에 대한 메서드를 Controller에 만들어준다.

  ```java
  public class QuestionController {
    ...
    @PutMapping("/{id}")
    public String update(@PathVariable Long id, String title, String contents) {
      Question question = questionRepository.findOne(id);
      question.update(title, contents);
      questionRepository.save(question);
      return String.format("redirect:/questions/%d", id);
    }
  }
  ```

  * `question.update()`가 Question 클래스 내부에 없기 때문에 만들어준다.

    ```java
    public class Question {
      ...
      public void update(String title, String contents) {
        this.title = title;
        this.contents = contents;
      }
    }
    ```

* 테스트

> 여기까지 실습

* 삭제기능을 구현해보자

  ```html
  <!-- show.html -->
  ...
  <form class="form-delete" action="/questions/{{id}}" method="post">
      <input type="hidden" name="_method" value="delete">
  </form>
  ```

  ```java
  public class QuestionController {
    @DeleteMapping("/{id}")
    public String delete(@PathVariable Long id) {
      questionRepository.delete(id);
      return "redirect:/";
    }
  }
  ```

* 테스트

> 여기까지 실습