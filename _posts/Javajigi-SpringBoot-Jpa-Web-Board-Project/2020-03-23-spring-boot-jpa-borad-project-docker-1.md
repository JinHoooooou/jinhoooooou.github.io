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

* docker toolbox를 설치한다. [Toolbox 설치](https://github.com/docker/toolbox/releases)

* 설치가 끝나면 window powershell에 `docker-machine`명령어로 확인할 수 있다.

  ![docker-machine]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-machine.png)

  * 설치 전에는 docker-machine이라고 입력해도 error만 나왔는데, 설치하고 나서 입력하면 명령어 실행이 된다.

* `docker-machine create --driver virtualbox default`로 도커 머신을 생성한다.

  * 강의에서는 virtualbox로 했는데 구글링을 해보니 Hyper-V가 더 성능이 좋다고 하여 Hyper-V로 진행했다.

  * `docker-machine create --driver hyperv default`를 입력하면 `Error with pre-create check: "no External vswitch found. A valid vswitch must be available..."`이라며 링크와 함께 에러가 발생한다. [링크](https://docs.docker.com/machine/drivers/hyper-v/")로 접속해 보자
  
  * Hyper V 관리자에서 `서버에 연결 - 로컬 컴퓨터` - `가상 스위치 관리자` - `새 가상 네트워크 스위치` - 에서 이름과 참고를 작성하고 생성한다.
  
    * 생성하고 재부팅하라고 하는데 재부팅하고나서 명령어를 입력하면 정상적으로 create가 된다. 명령어는 `docker-machine create -d hyperv --hyperv-virtual-swutch "Primary Virtual Switch" default`이다.
  
      * 여기서 Primary Virtual Switch는 새 가상 네트워크 스위치에서 생성한 이름이고, default는 임의로 정하는 name이다.
  
      ![docker-machine-create]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-machine-create.png)
  
    * create 처음할때 시간이 좀 걸리더라.
  
    * 삭제할 때는 `docker-machine rm default`를 입력하면 된다.
  
      ![docker-machine-rm]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-machine-rm.png)

* `docker-machine env default`를 입력하여 머신의 정보들이 나온다.

  ![docker-machine-env]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-machine-env.png)

* `& "C:\Program Files\Docker Toolbox\docker-machine.exe" env default | Invoke-Expression`도 입력하라고 메세지가 온다. 그대로 해본다.

  * 강의에서는 `docker images`명령어를 입력하면 docker daemon에 연결할 수 없다는 에러 메세지가 나온다. 그래서 `eval $(docker-machine) env default`를 입력 후 `docker images`를 입력하면 정상적으로 작동한다. 그거랑 같은 개념인거 같다.

  ![docker-machine-eval]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-machine-eval.png)

* `docker pull ubuntu`를 입력하여 우분투 image를 다운받을 수 있다.

* `docker run --name example ubuntu`로 example이름을 가진 우분투를 실행할 수 있다. `docker ps`로 실행중인 docker 이미지들을 확인할 수 있다.

  ![docker-run]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-run.png)

* 실행시킨 docker는 바로 stop이 되기 때문에 daemon으로 실행해야한다. 삭제하고 daemon으로 실행해보자.

  ![docker-run-daemon]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-run-daemon.png)

  * `docker rm example`로 삭제할 수 있다.
  * `docker -dit `옵션으로 데몬으로 실행할 수 있다.
  * stop, start하고 싶을 때는 `docker stop`, `docker start`를 입력한다. 

* 터미널에서 실행한 우분투로 접속해보자

  ![docker-exec]({{site.url}}/assets/images/2020-03-23-spring-boot-jpa-borad-project-docker-1.assets/docker-exec.png)

  * `docker exec -it example /bin/bash`로 접속한다.

  

