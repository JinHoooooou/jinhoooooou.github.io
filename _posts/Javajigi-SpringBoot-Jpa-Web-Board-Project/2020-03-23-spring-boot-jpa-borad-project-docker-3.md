---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(25)"
excerpt: "Docker3. Dockerfile을 활용해서 나만의 Docker 이미지 만들기"
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

### Docker3. Dockerfile을 활용해서 나만의 Docker 이미지 만들기

* 한글 버전 Docker container 설치 후 자바를 설치했었다.

* Git, Tomcat도 설치해야한다.

* Docker Container를 만들 때 마다 위 과정을 반복하는것은 좀 귀찮다.

  * 한글 설정을 위해 Dockerfile을 만들었었는데, 이미 자바가 설치된 Docker Container을 만들 수 있다.

* Docker Hub에서 java8 검색후 관련 docker container들을 pull 할 수 있는데 Dockerfile에서 FROM에 그대로 작성해주면 된다. ex) podbox/java8

* `docker build -t temp .`으로 빌드

* 혹은 docker search java8로 터미널에서 검색할 수 있다.

* Dockerfile에서 `RUN apt-get install -y \git \vim`등을 추가해서 빌드할때 바로 설치할 수 있도록 할 수 있다.

* Dockerfile을 이용하면 더 간편하게 배포환경을 세팅할 수 있다.

  
