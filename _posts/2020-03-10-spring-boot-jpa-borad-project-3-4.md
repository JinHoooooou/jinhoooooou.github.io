---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(15)"
excerpt: "개인정보 수정 기능 구현"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-10
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 3-4. 개인정보 수정 기능 구현

* navigation에 해당하는부분을 따로 추출해서 include해서 사용했었다.(중복제거)
* 메인페이지(index.html)와 회원가입페이지(signUpForm.html)도 static에서 templates으로 옮겼다.
* 이렇게 templates의 파일로 접근하기 위해서는 MVC패턴에 따라 controller로 먼저 접근을 해야한다.
  * 메인페이지는 HomeController를 이용했다.
  * 회원가입은 UserController를 이용했다.

> 여기까지 실습

* 개인정보 수정이라는것은 로그인을 한 상태에서 나의 정보를 수정하는것이다.

* 그런데 아직 로그인 기능을 구현하지 않았기 때문에 시스템 관리자라고 생각하고 사용자 조회를 한 상태에서 가입해있는 사용자를 수정하는 기능을 구현할 것이다.

* 사용자 목록에서 수정버튼을 눌렀을때 

  * 해당 사용자가 어떤 사용자인지 알아야한다. 그러므로 사용자에 해당하는 id를 서버에 전달해서 수정하고싶은 사용자의 데이터를 데이터베이스에서 조회해서 수정가능한 화면으로 이동해야한다.
  * 회원가입 화면과 비슷하게 구현될텐데 수정하고자하는 사용자의 정보가 이미 입력되어있기 때문에 form에 채워진 상태에서 수정이 가능하도록 하면 될것이다.
  * 그러므로 회원 수정 버튼에 url을 추가한다.

* UserController에 회원정보 수정을 위한 메소드를 추가한다.

  * updateForm()인데 인자로 회원정보를 받아야하는데 @PathVariable을 이용해서 받아올 수 있다.

  * 강의에서는 id번호로 했는데 userId로 해보자 그러면 Long이아닌 String으로 해야할거같다. 그리고 userRepository의 findOne도 다른걸 써야할거같다. 일단 둘다해보자

    > String으로 찾아보니 userRepository에 메소드를 추가해주어야한다 그래서
    >
    > `List<User> findByUserId(String userId)`를 추가해주니 알아서 된다!
  >
    > 추가! 그런데 지금 내가 연습하고 있는 테스트는 userId도 바꿀 수 있다. 그래서 primary key값인 id로 적용하는것이 낫겠다. 이런식으로 interface에 추가 구현을 할 수 있다는걸 이해했으니 됐다!

  * 조회된 사용자 정보를 모델에 담아서 뷰로 보내준다. 

  * 뷰(updateForm.html)의 form에 이전 정보들을 채워넣어야하는데 그때 사용하는 속성이 value이다. 그리고 User클래스에서 꺼내야하는것이므로 mustache문법으로 {{#user}}를 사용해야한다.

  * 그런데 css적용이 안되어있는데 상대경로로 입력해서 그렇다. 절대경로로 바꾸어주면 다시 적용된다.
  
  * 이 css부분도 중복이되기때문에 중복을 제거하기위해 header.html을 추가하여 중복을 제거해준다.

> 여기까지 실습

* 업데이트이기 때문에 회원가입과 다른 url을 사용해야한다.(회원가입은 /users였는데 회원정보는 /users/update이런식으로)

* input 태그를 이용, 속성은 type=hidden, name=id, value={{id}} 이 정보를 데이터베이스에 넘겨줘야 수정을 하거나 할 수 있다. 또는 그냥 url에 {{id}}를추가하면 굳이 hidden을 사용할 필요없다.

* 새 url이 추가되었으니 Controller에 메소드를 추가해주어야한다. POST방식이니 @PostMapping추가해주고, @PathVariable를 인자로 쓴다. 그리고 수정한 내용의 정보를 저장하기위한 User도 인자로 쓴다.

* userRepository를 이용해서 가입되어있는 회원정보를 가져와서 User 객체에 할당한 뒤 수정한다(이과정에서 update 메소드를 User클래스에 만들어준다.)

  * save메소드는 id가 기존에 있기때문에 추가하지않고 update한다
  * 그런데 update메소드를 User클래스에서 만들어서 사용하지않고, 내가 UserRepository 인터페이스에서 findByUserId메소드를 만든것처럼 Update도 만들 수 있지 않을까? 알아보기 위해서 JpaRepository에 대한 공부가 필요할것 같다. 찾아보자!

  > 있는거 같긴 한데, 아무리 해봐도 안된다.. 일단은 강의에서 하는대로 하고 내가 더 찾아보거나해야할듯.. ㅜㅜ

  * 생각해보면 primary key인 id를 기준으로 바꾸는데, userUpdateForm.html에서 id가 바뀌지 않고 그 값을 그대로 가져온다. 그러므로 UserRepoistory에서 굳이 가져오지않고, 그냥 update한 정보를 save하면 id는 그대로니까 새로 추가되는것이아닌 정보수정이 된다.

* 수정 기능은 두단계로 나뉜다.

  * 수정화면으로 넘어가는 기능
  * 수정화면에서 사용자가 데이터를 입력하여 submit하면 서버에서 그 데이터를 받아서 데이터베이스에 저장하는 기능
  * 웹 애플리케이션 개발과정에서 CRUD중에서 보통 수정기능이 가장 복잡하다고 한다. 따라서 수정기능의 flow만 이해해도 어느정도 잘 이해했다고 말할수있다.

* form태그에서 method 속성중 GET이랑 POST만 썼는데 사실은 PUT DELETE등 http 메소드가 있다. 일반적으로는 GET과 POST만 쓰는데, 수정기능은 PUT에 해당하기 때문에, PUT을 사용하도록 꼼수를 쓴다.

* form태그 안에 input태그를  type=hidden속성과 name=_method속성 그리고 value=put속성을 이용해서 PUT으로 감지한다. 그래서 Controller에서도 @PutMapping을 사용하여 "수정"기능을 구분 지을 수 있다.

* 그런데 안된다. 검색해보니 SpringBoot2버전부터는 Application에 bean을 추가해주어야한다고한다.

  ```java
  @SpringBootApplication
  public class MySlippApplication {
  
    public static void main(String[] args) {
      SpringApplication.run(MySlippApplication.class, args);
    }
  
    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter() {
      return new HiddenHttpMethodFilter();
    }
  }
  
  ```

  bean을 추가하고 해보니까 잘 된다.

> 여기까지 실습