---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(32)"
excerpt: "다섯번째 반복주기 원격 서버에 배포"
classes: wide
categories:
 - 웹 게시판 프로젝트
tags:
 - Java
 - Spring
 - JPA
 - javajigi
last_modified_at: 2020-03-30
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 5-7 다섯번째 반복주기 원격 서버에 배포

* 원격 서버에 접속한다.

* 이미 실행중인 이전 사이클때 실행한 서버를 종료한다.

  ```shell
  ps -ef | grep java
  ubuntu   12924     1  0  3월26 ?      00:05:29 java -jar my-slipp-1.0.jar
  ubuntu   22053 22046  0 05:14 pts/1    00:00:00 grep java
  kill 12924
  ```

* 그리고 배포한다. 이전 배포내용과 같다.