---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(33)"
excerpt: "AJAX를 활용한 답변 추가1"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-31
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 6-1. AJAX를 활용한 답변 추가 1

* 질문 상세보기에서 답변을 달았을 때, 추가되는 부분을 AJAX를 활용하여 구현 해볼 것이다.

* 실제로 구현해야 하는 부분이 답변하기 버튼을 클릭을 하면, 서버로 데이터가 전송이되는데 이  기능을 막은 다음에 ajax라는 기술을 통해서 서버에 전송하는 식으로 구현을 해야한다.

* show.html에 보면 댓글부분을 구현할 자바스크립트 영역은 footer.html에 포함되어있다. 그 중 jquery와 bootstrap은 외부 라이브러리고, scripts.js를 가지고 ajax를 구현해볼 것이다. 

* 답변하기 버튼을 클릭했을 때 서버로 데이터를 바로 전송하지 않도록 먼저 구현해보자.

  ```javascript
  $(".answer-write input[type=submit]").click(addAnswer);
  
  function addAnswer(e) {
    console.log("click me");
    e.preventDefault();
      
    var queryString = $(".answer-write").serialize();
    console.log("query : " + queryString);
    
    var url = $(".answer-write").attr("action");
    console.log("url : " + url);
    $.ajax({
      type : 'post',
      url : url,
      data : queryString,
      dataType : 'json',
      error : onError;
      success : onSuccess});
  }
  
  function onError() {
      
  }
  function onSuccess() {
      
  }
  ```

  * 기존에 작성되어있는 코드를 삭제하고 다시작성한것이다.
  * html의 class는 .으로 표현한다. `answer-write`라는 클래스를 가진 태그의 input type=submit이라는 자식 태그를 클릭 했을때, addAnswer라는 function을 호출한다.
  * 콘솔의 로그는 크롬 개발자도구의 console에서 확인할 수 있다.
  * addAnswer에 대한 function을 만든다.
    * 우선 submit을 했을 때 서버로 데이터로 보내는 default기능을 preventDefault()로 막는다.  
    * serialize()라는 function을 통해 `answer-write`의 textarea태그의 name="contents"데이터를 가져올 수 있다.
    * jquery의 ajax라는 function을 이용해서 서버에 데이터를 전송한다.
      * type - http method에 해당하는데 post방식이므로 post
      * url은 html의 form태그의 action에 해당하는 url을 가져갈 것이다. 따라서 url을 따로 `$(".answer-write").attr("action")`으로 추출한다.
      * data타입은 json, xml 등 있는데 json으로 선택한다.
      * 응답 결과가 error라면 onError, success라면 onSuccess function을 호출한다.

* 자바스크립트에 대한 show.html도 수정한다.

  ```html
  <!-- show.html -->
  ...
  <form class="answer-write" ... action="/api/questions/{{id}}/answers">
  </form>
  ```

* 자바스크립트에 대한 AnswerController를 수정해보자. 기존에 AnswerController를 사용하지 않고 json을 이용하여 데이터를 전달한다.

  ```java
  @RestController
  @RequestMapping("/api/questions/{questionId}/answers")
  public class ApiAnswerController {
    ...
    @PostMapping("")
    public Answer create(@PathVariable Long questionId, String, contents, HttpSession session) {
      if (!HttpSessionUtils.isLoginUse(session)) {
        return null;
      }
        
      Uesr loginUser = HttpSessionUtils.getUserFromSession(session);
      Question question = questionRepository.findOne(questionId);
      Answer answer = new Answer(loginUser, question, contents);
      return answerRepository.save(answer);
    }
  }
  ```

  * 우리가 응답할 데이터는 Answer 데이터베이스에추가되어있는 answer정보를 클라이언트에 받고싶은것이다. 데이터베이스에 저장되기전에는 데이터베이스의 id가 null인 상태인데 데이터베이스에 저장되면 id값이 부여가되고 결과물이 생성된다. 그 데이터를 받기위해 String 대신 Answer사용했다.
  * save메서드를 보면 리턴값이 인자로 전달한 answer를 그대로 리턴한다.
  * Spring MVC가 컨트롤러에 해당하는 메서드를 바로 알아서 json으로 변환을 해주는것은 아니다. 그렇게 하도록 하려면 @Controller대신에 @RestController를 사용해야하한다.

* 테스트
  * 테스트 할 때 console을 확인해보면 데이터와 url이 제대로 전달된것을 확인할 수 있다. 여기서 새로고침(F5)를 누르면 실제로 댓글등록도 되어있다.

> 여기까지 테스트

* 서버작업까지 끝냈으니 클라이언트 작업을 해야한다. 응답(response)로 받은 answer데이터를 추가되는 부분만 다시 html을 만들어 그려줄 필요가있다. 구현해보자.

  ```javascript
  //scripts.js
  ...
  function onSuccess(data, status) {
    console.log(data);
  }
  ```

  * data는 answer, status는 200 등 http 상태코드
  * 콘솔로 확인해 보니, 다른 데이터는 안보이는데 formattedCreateDate field만 가지고있다. 다른 field는 getter메서드가 없어서 그런것이다. getter메서드를 사용하면된다.
  * 그건 다음시간에 다음 post에 다룬다..