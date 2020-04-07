---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(36)"
excerpt: "질문 목록에 답변 개수 보여주기 기능 추가"
classes: wide
categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-04-06
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 6-4 질문 목록에 답변 개수 보여주기 기능 추가

* question 제목 왼쪽에 '8'로 고정되어있는 답변 개수를 출력해주는 기능을 추가해보자.
* 질문 데이터가 많으면, 많을수록 데이터 목록을 가져오는데 시간이걸린다. 근데 각 목록 데이터에 대해서 몇개의 답변이 달려있는지 매번 데이터베이스에서 조회하는 쿼리를 날려야 댓글 개수를 알 수 있다. `select count(*) from answer where question_id = 1`

* 글이 많아질수록 각 글에대해서 현재 답변개수를 확인하기 위한 쿼리를 날리는 횟수가 증가한다. 이것은 성능상 좋지않다. 그래서 성능개선을 위해서 답변을 쓰는 순간에 질문 테이블에 답변 개수를 증가시켜주면 된다. 답변이 삭제되면 답변 개수를 감소시키면된다.

* 따라서 question 클래스에 답변 개수 필드를 추가한다.

  ```java
  public class Question {
    ...
    @JsonProperty
    private Integer countOfAnswer = 0;
    ...
  }
  ```

  * default값을 null이 아닌 0으로 지정해준다.

> 여기까지 실습

* 답변이 추가되거나 삭제될 때 countOfAnswer의 값을 1증가하거나 1감소시키면된다.

  ```java
  public class ApiAnswerController {
    ...
    @PostMapping("")
    public Answer create(@PathVariable Long id, String contents, HttpSession session) {
      ...
      question.addAnswer();
      ...
    }
      
    ...
    @DeleteMapping("/{id}")
    public Result result(@PathVariable Long questionId, @PathVariable Long id, HttpSession session) {
      ...
      Question question = questionRepository.findOne(questionId);
      question.deleteAnswer();
      questionRepository.save(question);
      ...
    }
  }
  
  public class Question {
    ...
    public void addAnswer() {
      this.countOfAnswer++;
    }
    
    public void deleteAnswer() {
      this.countOfAnswer--;
    }
  }
  ```

* 테스트

> 여기까지 실습

* view도 수정해주자.

  ```html
  <!-- index.html -->
  ...
  <div class="reply" title="댓글">
      <i class="icon-reply"></i>
      <span class="point">{{countOfAnswer}}</span>
  </div>
  
  
  <!-- show.html -->
  <div class="qna-comment-slipp">
    <p class="qna-comment-count"><strong>{{countOfAnswer}}</strong>개의 의견</p>
    ...
  </div>
  ```

* countOfAnswer를 증가시키거나 감소시킬 때 지금은 하드코딩에 가까운형태로 구현했지만 jpa에 대한 추가공부를 하고 나면 자연스럽게 할 수 있을것이다.

> 여기까지 실습