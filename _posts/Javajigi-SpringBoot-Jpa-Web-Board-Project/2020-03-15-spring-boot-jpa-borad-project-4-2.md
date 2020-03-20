---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(19)"
excerpt: "로그인 상태에 따른 메뉴 처리 및 로그아웃"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-15
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 4-2. 로그인 상태에 따른 메뉴 처리 및 로그아웃

* 메뉴가 posts, 로그인, 회원가입, 로그아웃, 개인정보 수정이 있다.
  * 로그인, 회원가입은 로그인 하지 않았을때 있어야하는 메뉴이다.
  * 로그아웃과 개인정보 수정은 로그인을 했을때 있어야하는 메뉴이다.
  * 로그인 기능을 구현하지 않았기때문에 4개 다 유지해놓은거지 이제 바꿀것이다.
* navigation.html에 if else가 필요할것같다.
* 그 전에 테스트 때 마다 서버가 재시작되니 회원가입을 새로해야하는데 매우 귀찮다. 이 과정을 단축시킬 방법이 있을까?
  * spring data jpa initialize database로 검색하니 여러가지 나온다.
    * import.sql 파일을 classpath의 root에두면된다고한다.
    * src/main/resources 디렉토리에 import.sql파일을 만든다.
    * insert into user (user_id, user_password, user_name, user_email) values ('','','','');
    * 강의에서는 그냥 됐는데 실습해보니 에러가발생했다
      * 댓글을 참고하니 User 클래스의 @GeneratedValue의 strategy = IDENTITY attribute를 설정해주면 된다고한다. 정상적으로 작동한다.

> 여기까지 실습

* 이제 navigation.html을 수정해보자.
  * Controller에서 setSession을 통해 저장한 이름을 템플릿엔진으로 전달되는지 잘 모른다고한다.(그냥 될거라고 예상은하는데.. 한번해보죠 라고하심 ㅋㅋㅋ)
  * {{#repo}}{{/repo}}{{^repo}}{{/repo}}이런식으로 if else문 구현할 수 있다.
  * 테스트 해보니 적용이 되지않았다.
  * properties파일에 `spring.mustache.expose-session-attributes=true`를 추가해주어야한다. 테스트해보니 적용이 되었다.
* 로그아웃 기능 구현
  * url을 /users/logout으로 설정
  * Controller에 메소드 추가, 저장한 session을 제거해야한다. removeAttribute메소드를 통해 제거할 수 있다.

> 여기까지 실습

