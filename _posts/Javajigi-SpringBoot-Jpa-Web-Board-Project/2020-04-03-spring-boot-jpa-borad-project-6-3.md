---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(35)"
excerpt: "AJAX를 활용한 답변 삭제"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-04-03
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 6-1. AJAX를 활용한 답변 삭제

* 댓글 삭제 기능 구현을 해보자

* show.html의 script구문에 삭제버튼이 있기 때문에 기존 삭제 버튼을 수정한다.

  ```html
  <!-- show.html -->
  
  ...
  <li>
      <a class="link-delete-article" href="/api/questions/{{question.id}}/answers/{{id}}">삭제</a>
  </li>
  ...
  <script>
  ...
  <li>
      <a class="link-delete-article" href="/api/questions/{3}/answers/{4}">삭제</a>
  </li>
  </script>
  ```

  * link-delete-article 클래스에 대한 버튼 클릭을 했을 때 이벤트를 잡으면된다.

* scripts.js에 작성한다.

  ```javascript
  //scripts.js
  ...
  $(".link-delete-article").click(deleteAnswer);
  function deleteAnswer(e) {
    e.preventDefault();
    var url = $(this).attr("href");
    console.log("url :" + url);
    
    $.ajax({
        type : 'delete',
        url : url,
        dataType : 'json',
        error : function(xhr, status) {
          console.log("error");
        },
        success : function(data, status) {
          console.log("success");
        }
    })
  }
  ```

  * 삭제 버튼을 눌렀을 때 해당 url로 가지않고 url을 얻어오기만 한다. 
  * error, success에 대한 function을 따로 구현할 수 있지만 내부에서 구현하여 처리할 수도 있다.

* 테스트해보면 당연하지만 url오류가 난다. Controller에 Delete에 대한 메서드를 구현한다.

  ```java
  public class ApiAnswerController {
    ...
    @DeleteMapping("{id}")
    public Result delete(@PathVariable Long questionId, @PathVariable Long id, HttpSession session) {
      if (!HttpSessionUtils.isLoginUser(session)) {
          return Result.fail("로그인 하세요.");
      }
      Answer answer = answerRepository.findOne(id);
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      if (!answer.isSameWriter(loginUser)) {
        return Result.fail("자신의 글만 삭제할 수 있습니다.");
      }
      answerRepository.delete(id);
      return Result.ok();
    }
  }
  ```

* 테스트
  * 테스트 하면 콘솔의 데이터에 value(true or false), errorMessage(false일때만)이 출력될텐데 value가 true이면 삭제에 성공한것이다.
  * :raising_hand_man:이해 안되는점 : 댓글을 작성하고 바로 삭제를 누르면 405 error가 발생한다. 그런데 새로고침을 한번 하고 삭제를 누르면 정상적으로 작동한다. 그런데 왜 그런지는 잘 모르겠다.

> 여기까지 실습

* success에서 value가 true일때에 대한 처리를 해주자.

  ```javascript
  ...
  function deleteAnswer(e) {
    ...
    success: function(data, status) {
      console.log(data);
      var deleteBtn = $(this);
      var url = deleteBtn.attr("href");
      
      $.ajax({
          ...
          success: function (data, status) {
            console.log(data);
            if(data.valid) {
              deleteBtn.closest("article").remove();
            } else {
              alert(data.errorMessage);  
            }
          }
      })
    }
  }
  ```

  * show.html에서 this에해당하는 부분에서 가장 가까운 article을 삭제한다. 그런데 위의 this와 ajax내부의 this가 다르기 때문에 임시변수를 선언해서 고쳐준다.

> 여기까지 실습