---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(20)"
excerpt: "자기 자신에 한해 개인정보 수정"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-16
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 4-2. 자기 자신에 한해 개인정보 수정

* 로그인 후 개인정보수정 메뉴를 누르면 자신의 개인정보를 수정할 수 있도록 개선해보자
* 개인정보 수정 버튼을 눌렀을 때 어떤 사용자인지 알아야한다.
  * navigation.html의 url 변경

* 로그인 하면 세션정보가 저장되어있기 때문에 세션 바탕으로 url로 이동한다. 그런데 강의에서 model에 담은 정보와 session에 담은 정보가 이름이 같기 때문에 error가 발생했다. 
* 수정하고 테스트해보니 잘 돌아감

> 여기까지 실습

* 개인정보 수정할 때 메뉴 버튼을 눌러서 url을 보내 수정을한다.

* 근데 만약 로그인을 하지않아도 url에 localhost:8080/users/{{id}}/update입력하면 바로 수정화면으로 넘어간다. 문제가있다! 그래서 로그인을 하지 않았을때 해당 url접근을 허락하지 않으면된다.

  * 그래서 Controller의 update 메소드에서 session 체크를 해준다.

    * ```java
      Object sessionedUser = session.getAttribute("sessionedUser");
      if(sessionedUser == null) {
          return "redirect:/users/loginForm";
      }
      ```

* 그런데 위처럼 수정했을때, 내가 로그인만 하게 된다면 다른 사용자의 개인정보도 수정할 수 있게된다. 문제가 있다!  goToUpdateFormPage 메소드를 보면 {id}를 가져와서 해당 사용자의 정보를 조회하는데 이런 방식을 사용하면 안된다. 현재 로그인된 사용자의 정보를 데이터베이스에서 가져오도록 설정을 해야한다.

  * sessionedUser가 null이 아닐경우 User 객체로 type 캐스팅을 해주어야한다.

    * ```java
      Object tempUser = session.getAttribute("sessionedUser");
      if(tempUser == null) {
          return "redirect:/users/loginForm";
      }
      User sessionedUser = (User) tempUser;
      if(!id.equals(sessionedUser.getId())) {
          throw new illegalStateException("자신의 정보만 수정가능");
      }
      혹은
      파라미터의 id를 써서 findById하는것이 아닌 sessionedUser의 id를 가져오면된다.
      User user = userRepository.findOne(sessionedUser.getId());
      ```

* 백엔드 프로그래밍을 할 때 보안처리(예외처리) 고려를 많이해야한다. update하는 페이지로의 접근도 보안처리를 해야하고, update 페이지에서 실제 update를 할때도 보안처리를 해야한다.  update 메소드도 마찬가지로 위의 코드를 삽입

> 여기까지 실습