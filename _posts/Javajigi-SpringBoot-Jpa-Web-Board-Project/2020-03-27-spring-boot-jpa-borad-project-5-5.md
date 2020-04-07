---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(30)"
excerpt: "답변 추가 및 목록 기능 구현"
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

### 5-5 답변 추가(댓글) 및 목록 기능 구현

* 답변을 추가하고 답변 목록을 가져오는 기능을 구현해보자.

* User나 Question처럼 Answer 클래스를 생성한다.

  ```java
  @Entity
  @ToString
  @Getter
  @Setter
  public class Answer {
    @Id
    @GeneratedValue
    private Long id;
    
    @ManyToOne
    @JoinColumn(foreignKey = @ForeignKey(name = "fk_answer_writer"))
    private User writer;
  
    @ManyToOne
    @JoinColumn(foreignKey = @ForeignKey(name = "fk_answer_to_question"))
    private Question question;
      
    @Lob
    private String contents;
    
    private LocalDateTime createDate;
    
    public Answer() {}
    public Answer(User writer, String contents) {
      this.writer = writer;
      this.contents = contents;
    }
  }
  ```

  * 컨텐츠같은경우 default로 String으로하면 데이터베이스에서 column길이가 255자이다. 그래서 @Lob 애노테이션을 붙여 255자 이상 작성할 수 있다.
  * answer는 항상 question에 종속되어있다. question이 있어야 answer도 있을 수 있다. 관계를 생각해보면 question하나에 여러 answer가 달릴 수 있으므로 many - to - one 관계이다.

* Answer클래스를 만들었으니 데이터베이스 역할을해줄 repository interface를 만든다.

  ```java
  public interface AnswerRepository extends JpaRepository<Answer, Long> {
    
  }
  ```

* 또한 AnswerController도 만들어준다.

  ```java
  @Controller
  @RequestMapping("/questions/{questionId}/answers")
  public class Answer {
    
  }
  ```

  * url을 생각해볼때, answer는 question과 {id}가 필요하다. 물론 question이 필요없을 수 있지만 question과 answer가 종속 관계이므로 url을 이런식으로 만드는것이 좋은 설계이다.

* show.html에서 답변하는 form이 있는데 그 부분에서 action url을 설정해주자.

  ```html
  <!-- show.html -->
  ...
  <form class="submit-write" method="post" action="/questions/{{id}}/answers">
      ...
        <textarea class="form-control" placeholder="댓글" name="contents"></textarea>
  <input type="submit" class="btn btn-success pull-right" value="답변하기" />
  </form>
  ```

* show.html에서의 `name="contents"`를 AnswerController에 매핑해주면

  ```java
  @Controller
  @RequestMapping("/questions/{questionId}/answers")
  public class Answer {
    @Autowired;
    private AnswerRepository answerRepository;
    
    @PostMapping("")
    public String create(@PathVariable Long questionId, String contents, HttpSession session) {
      if (!HttpSessionUtils.isLoginUser(session)) {
        System.out.println("Fail about go to update form page : Login failure");
        throw new IllegalStateException("로그인 하세요");
      }
      User loginUser = HttpSessionUtils.getUserFromSession(session);
      Answer answer = new Answer(loginUser,contents);
      answerRepository.save(answer);
      return String.format("redirect:/questions/%d", questionId);
    }
  }
  ```

* 테스트

  * 아직 view화면에는 출력이 되지 않았지만 데이터베이스상으로는 데이터가 들어온것을 확인할 수 있다. 
* question과 create_date가 null값이 들어온다.
  
* Answer 클래스의 Constructor의 인자로 Question 클래스를 추가한다.

  ```java
  public class Answer {
    ...
    public Answer(User writer, Question question, String contents) {
      this.writer = writer;
      this.question = question;
      this.contents = contents;
      this.createDate = LocalDateTime.now();
    }
  }
  ```

* AnswerController도 Answer Constructor의 인수로 Question클래스를 추가해준다.

  ```java
  public class AnswerController {
    @Autowired
    private QuestionRepository questionRepository;
    ...
    @PostMapping("")
    public String create(@PathVariable Long questionId, STring contents, HttpSession session) {
      ...
      Question question = questionRepository.findOne(questionId);
      Answer answer = new Answer(loginUser, question, contents);
    }
  }
  ```

* 테스트

* 질문의 추가되어있는 답변의 목록을 가져오고 그 목록을 출력할 수 있어야한다. 보통 questionRepository를 통해서 현재 questionId를 가져오고 그것을 통해 답변 목록을 가져올 수 있다.

  ```java
  public class QuestionController {
    ...
    @GetMapping("/{id}")
    public String show(@PathVariable Long id, Model model) {
      ...
      answerRepository.findsByQuestionId()
    }
  }
  ```

* 이렇게 할 수도 있지만 Question클래스에서 Answer클래스를 가지고 있도록 할 수도 있다.

  ```java
  public class Question {
    ...
    @OneToMany(mappedBy="question")
    @OrderBy("id ASC")
    private List<Answer> answers;
  }
  ```

  * Question입장에서 Answer는 one to many관계이므로 어노테이션을 붙여준다. Question클래스에서 many to one으로 answer과 매핑했으므로 그 필드 이름(question)을 써주면된다.
  * OrderBy로 정렬 기준과 오름차순,내림차순 정렬을 지정할 수 있다.
  * 그러면 QuestionController의 show 메서드에서 answerRepository를 통해 answer목록을 가져올 필요없이 question객체에 answer목록이 담겨져있다.

* show.html에서 댓글목록을 출력할 수 있다.

  ```html
  <!-- show.html -->
  ...
  <div class="qna-comment-slipp-articles">
      {{#answers}}
      <article class="article" id="answer-1405">
        ...
          <a href= ...>{{writer.userId}}</a>
          <a href=...>{{formattedCreateDate}}</a>
          ...
          <p>{{contents}}</p>
      </article>
      {{/answers}}
  </div>
  ```

