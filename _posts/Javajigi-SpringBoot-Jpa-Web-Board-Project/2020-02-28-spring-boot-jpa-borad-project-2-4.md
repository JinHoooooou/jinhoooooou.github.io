---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(9)"
excerpt: "회원가입, 사용자 목록 출력 기능 원격 서버에 배포"

categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-02-28
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 2-4. 회원가입, 사용자 목록 출력 기능 원격 서버에 배포

* 회원 가입, 사용자 목록 출력 기능을 원격 서버에 배포

* 원격서버에서 깃헙을 통해 내가 개발한 코드를 가져왔었다

* 그러므로 내가 로컬에서 개발한 코드를 깃헙에 업로드해야한다.

* 1번 반복주기에서 했던 내용 그대로 하면된다.

* 나는 브랜치를 새로 만들어서 작업했기때문에 git pull이 아닌 git checkout -t origin/브랜치명 으로 브랜치를 가져와서 진행했다.

  ./mvnw clean package를 하면 clean먼저하고 package를 수행한다.
  
  그래서 clean하면 target디렉토리가 날아가고, package는 jar파일을 만드는 작업을 수행한다.