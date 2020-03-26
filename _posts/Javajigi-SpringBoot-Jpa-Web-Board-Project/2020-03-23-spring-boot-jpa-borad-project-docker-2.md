---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(24)"
excerpt: "Docker2. docker ubuntu 한글버전 및 자바 설치"
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

### Docker2. docker ubuntu 한글버전 및 자바 설치

* Docker의 ubuntu 한글버전을 만들어 볼 것이다. 그리고 그 ubuntu에 자바를 설치한다.

* dockerfile을 생성 후 다음과 같이 작성한다

  ```shell
  FROM ubuntu:latest
  
  RUN apt-get update
  
  RUN apt-get install -y language-pack-ko
  
  #set locale ko_KR
  RUN locale-gen ko_KR.UTF-8
  
  ENV LANG ko_KR.UTF-8
  ENV LANGUAGE ko_KR.UTF-8
  ENV LC_ALL ko_KR.UTF-8
  
  CMD /bin/bash
  ```

* `docker build --tag ko_ubuntu:latest ./`로 ubuntu 빌드

  * docker images로 빌드 확인

* `docker run -dit --name my-slipp ko_ubuntu `로 docker 실행

  * docker ps -a로 확인

* `docker exec -it  /bin/bash`로 빌드한 한글 ubuntu로 접속

* 자바설치는 [1-4]({{site.url}}/_post/Javajigi-SpringBoot-Jpa-Web-Board-Project/2020-02-26-spring-boot-jpa-borad-project-1-4.md)에서 확인가능

  
