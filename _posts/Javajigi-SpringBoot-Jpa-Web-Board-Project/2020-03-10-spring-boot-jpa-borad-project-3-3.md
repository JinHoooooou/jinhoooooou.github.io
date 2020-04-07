---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(14)"
excerpt: "HTML과 URL 정리, 리팩토링"

categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-03-10
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 3-3. 새로운 HTML 적용 및 URL 리팩토링

* 개인정보 수정 기능 구현을 하려고 했는데 정리(리팩토링을 하는게 먼저인거 같다)
* HTML template을 가져왔는데 수정하고 코드정리가 필요해서 먼저 하고 다음단게로 넘어가려한다.



* H2 DB를 설치했는데, db파일 경로를보면 ~/my-slipp으로 ~디렉토리에 존재한다.
* 이전까지 코드로 봤을때, 회원가입 경로가 localhost:8080/signUpFrom.html이었다. 근데 이 url을 새로 적용한 html template의 user/form.html으로 옮겨볼것이다.
  * form.html파일을 보면 form 태그의 method 속성이 GET방식이다. 이것을 POST로 바꾸어주어야한다.
  * 그래서 Controller에서도 URL을 /create를 /user/create로 고쳐주어야한다. list도 마찬가지로 /user/list로 고쳐주어야한다.
* template 디렉토리에 user 디렉토리를 만들어 list.html을 그쪽으로 옮겨준다.
  * url 수정을 해주고 테스트 후 확인
  * User 클래스에서 만든 primary key ID값도 적용
* 필요없는 파일들 삭제

> 여기까지 실습진행

* url에 대한 고민을 최소화하기위해 패턴을 사용해야한다. 새로운 사용자를 추가한다. 추가할때 메소드를 get post가 있었는데 post를 사용, post를 사용하면서 /user 라는 url을 사용하면 user에 대한 로직이라는것을 파악할 수 있다. 그래서 대표 user에 대한 url매핑을 해준다. 회원가입의 /users와 회원목록 조회의 /users url은 같으나 GET방식 POST방식 다르기때문에 괜찮다.
* url에 대한 고민을 많이하게 될텐데 어떠한 패턴을 스스로 정하거나, 기업 등에서 사용하는 패턴을 그대로 사용하는것도 좋은 방법이다.
* /users라는 url을 기능이 추가될때마다 url이 더 추가되거나 변경될 수 있기 때문에 중복을 제거할 수 있도록 @RequestMapping을 사용해서 대표 url을 설정할 수 있다.
* 또 중복 제거관련 코드는 상단 네비게이션바는 html이 똑같다.  이 부분 중복코드를 따로 directory를 파서 하나로 묶을 수 있다.
  * html은 정적이기 때문에 중복제거가 힘들고, mustache를 이용하여 중복코드를 제거할 수 있다.
  * templates 디렉토리에 include 디렉토리를 생성후 navigation.html파일을 생성하여 중복코드 복사
  * 파일을 접근하는 방식이 templates 디렉토리를 root로 본다. 그래서 네비게이션부분을 {{> /include/navigation}}으로 작성하여 적용한다.

> 여기까지 실습진행