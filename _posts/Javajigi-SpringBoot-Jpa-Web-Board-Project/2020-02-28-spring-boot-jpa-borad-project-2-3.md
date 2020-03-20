---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(8)"
excerpt: "사용자 목록 기능 구현"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-02-28
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 2-2. 사용자 목록 기능 구현

* 회원가입이 되었으면 회원가입된 유저들 목록을 보여준다.

* 사용자 목록을 보여주려면 서버에서 회원가입된 사용자들의 데이터를 저장하고있어야한다.

* 그래서 원래는 DB를 사용하지만 지금은 간단하게 일단 List로 저장한다.

* signUpUser method에서 회원가입이 된 유저들의 정보를 List에 추가한다.

* list를 출력하는 페이지가 필요 html파일 생성

* mustache 템플릿을 이용해 list에 저장된 데이터들을 반복문으로 출력

  * {{#}}

    ​	{{}}

    {{/}}

* 회원가입 완료 후 바로 사용자 목록으로 이동 할 수 있도록 수정 - redirect

  > 처음에 그냥 회원가입 완료되고 list로 이동하게 하면 되지 않나 생각했는데 그러면 list라는 url이 회원가입 이후에만 볼 수 있는 페이지로 바뀐다. list url을 두고 회원가입 완료 후 그 페이지로 redirect하도록 만드는게 맞네. 