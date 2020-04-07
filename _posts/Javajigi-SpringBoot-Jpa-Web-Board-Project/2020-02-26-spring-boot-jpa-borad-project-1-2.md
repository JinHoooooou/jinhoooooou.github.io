---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(2)"
excerpt: "Bootstrap을 활용한 HTML 페이지 개발"

categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-02-26
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 1-2. bootstrap을 활용한 HTML페이지 개발

부트스트랩 start html 추가

부트스트랩 css 라이브러리 추가

jquery라이브러리  추가

1. bootstrap.com - get started - Hello world template 적용해보기

   * 적용해보고 localhost:8080로 접속, 개발자도구 - Network탭 - 적용되었는지 확인

   > 강의에서는 적용이 안된걸로 되어있는데 나는 되어있다 버전때문에 그런거라고생각함
   >
   > 2~4 는 강의에만 나온 내용이고 나는 안함(적용이 바로 되어있으니까)

2. mvnrepository.com으로 가서 bootstrap검색 - maven dependency를 pom.xml에 적용
3. 적용한 dependency경로를 index.html에 적용
4. 적용한 dependency확인은 External Library에서 확인(eclipse에서는 Maven Dependency에서 확인가능했음)
5. 다시 부트스트랩으로 가서 - 네비게이션 바 적용
6. 네비게이션에서 필요없는 부분 삭제(Link, dropdown, form 등)
7. navbar에서 회원가입을 위한 link설정 후, 경로를 form.html로 지정
8. form.html 페이지 개발 - 부트스트랩에서 form가져와서 적용
9. 결론 : 부트스트랩 라이브러리를 활용하여 html페이지를 쉽게 구성할 수 있다. 우리는 디자이너가 아니기 때문에(물론 디자인에 관심잇으면 그래도되지만) 가져다써서 좀더 편하고 쉽게 html페이지를 꾸밀 수 있다.