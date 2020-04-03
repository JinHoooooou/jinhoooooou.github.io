---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(34)"
excerpt: "AJAX를 활용한 답변 추가2"
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

### 6-1. AJAX를 활용한 답변 추가 2

* 응답(response)을 json으로 받는 api를 구현해야하는데 지금 눈으로 바로 확인하기 힘들다. 그래서 사용자 정보를 조회하는 controller를 추가한다음 브라우저상에서 해당하는 사용자의 정보를 json으로 받아보는 기능을 추가해보자.

  ```java
  @RestController
  @RequestMapping("api/users")
  public class ApiUserController {
    @Autowired
    private UserRepository userRepository;
    
    @GetMapping("/{id}")
    public User show(@PathVariable Long id) {
      return UserRepository.findOne(id);
    }
  }
  ```

  * 해당 url을 입력하면 json형태의 user정보를 받아올 수 있다.
  * :raising_hand_man:실제로 테스트했을때 계속 error가 발생했다. 구글링을 해본 결과 나는 UserRepository의 findOne()메서드를 사용하면 컴파일 오류가 났기 때문에 이전까지 findById().get()메서드를 사용했다. 근데 메서드 목록을 보니 getOne()메서드가 있었다. 그래서 findById().get()메서드 대신 getOne()메서드를 계속 사용해왔었는데, 결과적으로 이것 때문에 error가 발생했다.
    * [https://blogdeveloperspot.blogspot.com/2018/12/spring-boot-2-jpa-no-serializer-found.html](https://blogdeveloperspot.blogspot.com/2018/12/spring-boot-2-jpa-no-serializer-found.html)

> 여기까지 실습

* Getter 메서드를 만드는것도 좋지만, @JsonProperty를 추가해줘도 된다. 무시하고 싶은 field가 있다면 @JsonIgnore를 추가해주면된다.

  ```java
  public class User {
    ...
    @JsonProperty
    private Long id;
    @JsonProperty
    private String userId;
    @JsonIgnore
    private String password;
  }
  ```

  * Answer도 마찬가지로 추가해준다.
  * :raising_hand_man: 나는 lombok의 getter를 사용했기 때문에 따로 사용하진않았다.
  * :raising_hand_man: 그냥 사용하는게 좋을것 같아 사용했다 ㅋㅋㅋ..
  
* 클라이언트 템플릿 사용

  ```javascript
  //scripts.js
  ...
  String.prototype.format = function() {
    var args = arguments;
    return this.replace(/{(\d+)}/g, function(match, number) {
      return typeof args[number] != 'undefined'
        ? args[number]
        : match
        ;
    });
  };
  ```

* template 파일은 qna/show.html에 있다. 복붙

  ```html
  <!-- show.html -->
  </div>
  ...
  <script type="text/template" id="answerTemplate">
  	<article class="article">
  		<div class="article-header">
  			<div class="article-header-thumb">
  				<img src="https://graph.facebook.com/v2.3/1324855987/picture" class="article-author-thumb" alt="">
  			</div>
  			<div class="article-header-text">
  				<a href="#" class="article-author-name">{0}</a>
  				<div class="article-header-time">{1}</div>
  			</div>
  		</div>
  		<div class="article-doc comment-doc">
  			{2}
  		</div>
  		<div class="article-util">
  		<ul class="article-util-list">
  			<li>
  				<a class="link-modify-article" href="/api/qna/updateAnswer/{3}">수정</a>
  			</li>
  			<li>
  				<form class="delete-answer-form" action="/api/questions/{3}/answers/{4}" method="POST">
  					<input type="hidden" name="_method" value="DELETE">
                       <button type="submit" class="delete-answer-button">삭제</button>
  				</form>
  			</li>
  		</ul>
  		</div>
  	</article>
  </script>
  ...
  ```

* script의 onSuccess function에서 데이터를 받을것이다.

  ```javascript
  ...
  function onSuccess(data, status) {
    console.log(data);
    var answerTemplate = $("#answerTemplate").html();
    var template = answerTemplate.format(data.writer.userId, data.formattedCreateDate, data.contents, data.id, data.id);
    $(".qna-comment-slipp-articles").append(template);
  }
  ```

  * show.html의 데이터를 가져오기위해 $("#answerTemplate")을 사용, class는 . id는 #을 사용한다.
  * answerTemplate.format()은 클라이언트 템플릿을 사용하기위해 작성한 String.prototype.format을 사용하는것이다.
  * template이라는 변수에 userId, formattedCreateDate, contents, id, id가 전달이되어서 show.html의 script부분의 {0}, {1}, {2}.. 에 저장한다고 보면된다.
  * prepend()와 append()가 있는데 append는 뒤에 붙이는것, prepend는 앞에 붙이는것이다.

* 테스트

  * 댓글을 작성하고 답변하기 버튼을 누르면 댓글은 잘 생성되는데, 내가 작성했던 댓글이 그대로 남는 문제가 있다.

> 여기까지 실습

* 댓글이 남아있는 현상을 지워보자

  ``` javascript
  //script.js
  ...
  function onSuccess(data, status) {
    ...
    $("textarea[name=contents]").val("");
  }
  ```

> 여기까지 실습

* :raising_hand_man: 강의에서는 댓글을 남기고 나면 댓글 남긴 userid에 link가 생기지 않았는데 나는 있다 왜 그런지는 잘 모르겠다.
* :raising_hand_man:강의에서는 prepend를 사용했고 나는 append를 사용했다. 그런데 append를 사용하니까 댓글을 남기는 form 아래에 댓글이 생성되서 이상해보인다.
  * 그래서 show.html의 댓글 남기는 form을 qna-comment-slipp-articles class 밖으로 빼니까 정상적으로 됐다.