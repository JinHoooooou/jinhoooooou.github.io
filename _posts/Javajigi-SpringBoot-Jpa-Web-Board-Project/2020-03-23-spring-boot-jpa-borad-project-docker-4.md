---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(26)"
excerpt: "Docker4. 소스코드 Docker에 배포하기"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-23
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### Docker4. 소스코드 Docker에 배포하기

* 이전까지 docker 환경 세팅을 토대로 소스코드를 배포해보자
* docker 실행을 한다.
  * 브라우저를 통해 docker 컨테이너에 직접 접근 할 수 없다. 그 앞단에 docker-machine이라는것이 있기때문에 docker-machine의 특정 포트에 요청을 보내서 docker container쪽으로 연결하라고 포트 설정을 할 수 있다. 옵션 -p 
  * `docker run -dit --name my-slipp -p 7000:8080 my_dev` 앞의 7000이 docker machine포트번호이고, 뒤의 8080이 소스코드를 배포할 포트이다.
    * 7000번포트로 요청이 오면 8080으로 보낸다라는 뜻
  * `docker exec -it my-slipp /bin/bash`으로 원격 서버 접속
* `git clone https://github.com/JinHoooooou/SpringBoot-JPA-webBoard.git`으로 원격 repo 가져온다.
* 그 이후는 AWS EC2과정과 같다.
  * clone한 디렉토리로 이동하여 `git checkout -t <remote branch>`를 입력하여 배포하려는 branch를 가져온다.
  * `chomod +x mvnw` - `./mvnw clean package`로 빌드
  *  target 디렉토리로 이동하여 `java -jar <file>.jar`로 실행
* 7000을 8080으로 매핑했기때문에 docker-machine ip:7000을 입력하면 접속할 수 있다.
