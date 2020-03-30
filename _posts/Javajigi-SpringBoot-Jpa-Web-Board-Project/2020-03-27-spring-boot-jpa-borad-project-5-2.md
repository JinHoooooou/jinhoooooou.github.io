---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(29)"
excerpt: "질문 상세보기 기능"
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

### 5-2 질문 상세보기 기능

* 질문의 목록을 출력하는 기능까지는 구현했으니, 그 목록 중 하나를 클릭했을대 상세 페이지로 갈 수 있도록 구현해보자.

* index.html의 링크를 static이 아닌 template으로 수정한다.

  ```html
  <!-- index.html -->
  ...
  <a href="/questions/{{id}}">{{title}}</a>
  ```

* link를 static에서 templates으로 수정해주었으니 당연히 Controller에 메서드를 추가한다.

  ```java
  public class QuestionController {
    ...
    @GetMapping("/{id}")
    public String show(@PathVariable Long id, Model model) {
      model.addAttribute("question", questionRepository.findOne(id));
      return "qna/show";
    }
  }
  ```

* static/qna 디렉토리의 show.html을 templates로 옮기자.

  * show.html도 다른 html파일처럼 header, footer, navigation 정리하자.

* show.html에서 mustache template을 사용하여 상세페이지 수정

  ```html
  <!-- show.html -->
  ...
  <div class="col-md-12 col-sm-12 col-lg-12">
    {{#question}}
    ...
      <h2 class="qna-title">{{title}}</h2>
    ...
      <a href = "/users/92/kimm" class="article-author-name">{{writer.userId}}</a>
      <a href = "/question" class="article-header-time" ...>
          {{formattedCreateDate}}
      </a>
      ...
      <p>
          {{contents}}
      </p>
    {{/question}}  
  </div>
  ```

* 테스트
  * 입력한 contents가 그대로 출력된다. html태그를 일부러 적어놨는데 적용이 안되고 입력한 그대로 출력된다. 이것을 escaping이라고하는데 이런 처리를 해주어야 보안상 문제가 발생하지 않는다. 근데 줄바꿈(enter)가 적용되지 않기 때문에 나중에 Util클래스를 만들어 enter에 해당하는 부분을 html의 br역할을 하도록 작업을 할 필요가 있다. 

> 여기까지 실습