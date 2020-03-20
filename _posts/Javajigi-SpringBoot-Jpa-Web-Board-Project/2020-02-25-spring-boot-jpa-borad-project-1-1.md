---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(1)"
excerpt: "Spring Boot 로컬 개발 환경 세팅"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
last_modified_at: 2020-02-25
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 1-1. Spring Boot 로컬 개발 환경 세팅

1. IntelliJ를 통해 Spring boot 프로젝트 생성

   New Project - Spring Initializer - Group, Artifact, name, version등 세팅 - Next

   Dependency - DevTools, Web, Mustache 선택 후 build

2. Build가 끝나면 상단의 Run 버튼 (Shift + Alt + X)을 눌러 Boot 실행

3. chrome으로 localhost:8080확인

4. 페이지가 없기때문에 error페이지생성

5. html파일 추가 - src/main/resources/static에 index.html파일 생성

6. index.html파일에 Hello World 작성

7. 영상에서는 rerun으로 재시작하지않아도 출력되는데 intellij에서는 재시작을해야하네??

   > 이거 방법 찾아보기
   >
   > 1. Setting - Build, Execution, Deployment - Compiler 탭에서 Build project automatically 체크
   > 2. Ctrl + Alt + Shift + / 누르면 나오는 메뉴에서 Registry 선택 후 compiler.automake.allow.when.app.running 체크
   > 3. intelliJ 재시작 후 확인 OK

8. live reload 크롬 확장프로그램 설치 (이건 그래도 아는거라 넘어가도됨)
   * live reload를 사용하려면 DevTools dependency가 꼭 필요하다.





 

