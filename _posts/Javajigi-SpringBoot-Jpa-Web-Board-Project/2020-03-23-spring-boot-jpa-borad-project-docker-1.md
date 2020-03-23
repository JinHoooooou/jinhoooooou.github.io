---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(23)"
excerpt: "Docker1. docker Container를 실행하기 위한 환경 설정"
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

### Docker1. docker container를 실행하기 위한 환경 설정

* 소스코드들을 배포할 때, AWS나 Microsoft Azure같은 클라우드 서비스가 많기 때문에 서버에 소스코드를 배포하는 실습을 쉽게 할 수 있다.
  * 무료로 사용할 수도 있지만 일정기간이 지나면 돈을 내야하기도 하고, 내가 전체를 control할 수 없기 때문에 불편한점이 다소 있다.
  * 이런것이 불편해서 로컬pc나 노트북에 클라우드 서비스 환경을 구축하고 거기에 우분투를 설치해 원격서버 배포를 할 수 있다.
* docker는 가벼운 클라우드 서비스이며, 로컬에서 바로 진행할 수 있다.
* 강의에서는 docker toolbox를 설치한다고 되어있는데 현재는 docker toolbox가 아니라 docker desktop설치이므로 그 과정을 설명한다.
* 준비단계
  * Windows 10 x64 pro, Enterprise 또는 Education이 필요하다
  * CPU 가상화 기능이 활성화 되어야한다. 작업 관리자의 성능 탭에서 확인할 수 있다.
  * Windows 기능 켜기/끄기 기능에서 Hyper-V옵션을 활성화 해야한다.
* Docker 설치
  * [Docker 설치](https://docs.docker.com/docker-for-windows/install/)에서 설치
  * 설치가 완료되면 재시작
  * ![icon-tray]({site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/icon-tray.png)
    * 아이콘과 트레이 화면

* 세팅

  * [회원 가입](https://hub.docker.com/?utm_source=docker4win_2.2.0.4&utm_medium=account_create&utm_campaign=referral), 로그인

  * 트레이에서 `Switch to Linux containers`눌러서 변환

    ![switch-ubuntu]({site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/switch-ubuntu.png)

    ![before-after]({site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/before-after.png)

* `docker pull ubuntu`로 ubuntu 설치

* `docker run -dit --name dockerForJwp ubuntu`명령어로 데몬 실행

  * docker Setting-General에서 `Expose daemon on tcp://localhost:2375 without TLS`를 체크해야만 가능
  * `docker ps -a`나 desktop에서 확인가능

  ```powershell
  PS C:\Users\User> docker ps -a
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  daec367c1545        ubuntu              "/bin/bash"         31 minutes ago      Up 30 minutes                           dockerForJwp
  PS C:\Users\User>
  ```

  ![docker-desktop]({site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-desktop.png)

* `docker stop dockerForJwp`나 `docker start dockerForJwp`로 실행, 중지 가능
* `docker exec -it dockerForJwp /bin/bash`로 ubuntu접속 가능

