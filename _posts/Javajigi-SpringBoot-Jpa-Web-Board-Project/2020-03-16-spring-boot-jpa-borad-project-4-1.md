---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(18)"
excerpt: "로그인 기능 구현"

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

### 4-1. 로그인 기능 구현

* 로그인 화면 url을 보면 아직 .html이 붙어있다. 즉 static 디렉토리에 존재한다는 얘기
* login.html의 head태그와 nav태그로 중복되는 부분이 매우 많다. templates 디렉토리의 중복 제거한것처럼 만들것이다.
* 항상 생각할것! templates 디렉토리의 view로 전달하기위해선 controller에서 메소드를 추가해주어야한다.
* header뿐 아니라 html 아랫부분도 중복이 많기때문에 footer를 만들어 중복을 제거해보자.
  * 중복을 제거하는 연습이 필요하다. 그래야 코드가 깔끔
* navigation.html의 url까지 수정해주고 확인해보면 완료

> 여기까지 실습

* 로그인 할 때 생각을해보면 id, 패스워드 정보가 전달되기 때문에 post method가 필요하다. @PostMapping사용
  * form태그내부의 input태그의 name 속성의 값을 통해 전달된다.

* 인자를 통해 전달된 데이터가 데이터베이스에 존재해야한다. userRepository의 find메소드를 통해 찾을 수 있는데, userId를 확인해야하기 때문에 userRepository interface에서 새 메소드를 생성해준다.
* 유저 아이디가 데이터베이스에 존재하지 않는다면 다시 loginForm.html로 보내고 존재한다면 password가 일치하는지 체크한다. 마찬가지로 일치하지않는다면 loginForm.html으로 보낸다.
* 로그인이 성공했다면 세션을 통해 로그인한 사용자 정보를 저장한다. HttpSeesion을 통해 저장할 수 있다. setAttribute 메소드를 통해 저장할 수 있다.
* 테스트, 로그 남겨서 확인까지
  * 로그를 찍어놓는것이 어디가 문제인지 확인하기 편하다.(흔히 디버그로그라고한다)
  * 근데 이 방법은 치명적인 문제가 있는데 추후에 logging 라이브러리를 통해 개선해보자.

> 여기까지 실습