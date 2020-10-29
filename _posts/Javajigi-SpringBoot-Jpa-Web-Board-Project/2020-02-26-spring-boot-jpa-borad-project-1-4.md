---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(4)"
excerpt: "원격 서버에 소스코드 배포하기"

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

### 1-4. 원격 서버에 소스코드 배포하기

1. aws 계정 생성(프리티어, 학생권한은 아직 못얻음 ㅜㅜ) - ec2 instance 생성 - git bash이용해서 원격서버 로그인

   > 생활코딩 aws검색해서 참고하면서 했음 나중에 aws관련도 정리하자

2. ssh로 원격서버에 접속하여 계정추가 adduser명령어사용하여 계정생성

   > 집에서 연결하고 종료후에 나중에 다른곳에서 연결하려면 time out error가 발생한다.
   >
   > 이유를 찾아보니 보안그룹 인바인드에 ssh 할당 ip가 집으로 되어있기 때문
   >
   > 편집을 눌러 소스탭에서 내 ip를 선택하고 업데이트해주자 그러면 잘 된다.
   >
   > stop후 run하고나서 이것도 안된다 그래서 결국 terminate하고 다시생성했다. --> ufw문제였다.

3. 추가한 계정에 sudo권한부여 (vi /etc/sudoers)

   > 2~3은 aws기준으로 이미 ubuntu 계정이있기때문에 걸러도된다.

4. 각 계정별로 UTF-8 인코딩 설정

   ```shell
   $ sudo locale-gen ko_KR.EUC-KR ko_KR.UTF-8
   $ sudo dpkg-reconfigure locales
   ```

   home 디렉토리에 .bash_profile 생성 후

   LANG="ko_KR.UTF-8"

   LANGUAGE="ko_KR:ko:en_US:en"

   ```shell
   $ source .bash_profile
   ```

   env명령어로 설정확인

5. 자바 설치

   jdk 1.8 다운 검색 - 설치 - 라이센스 확인 - 설치링크 복사

   원격서버에서 라이센스확인을 누를 수 없음 --> 명령어로 해결

   `wget --header "Cookie: oraclelicense=accept-securebackup-cookie" 설치링크`

   ls로 설치 확인

   > ls가 색상이 안나온다.. 왜이러지해서 찾아보니
   >
   > .bashrc 파일에 설정을해주어야한다
   >
   > export CLICOLOR=1
   >
   > export LSCOLOR=DxFxBxDxCxegedabagacad

   받은 파일 압축해제 `gunzip jdk.tar.gz`

   **근데 이방법으로 시간 존나날려먹었다. gz파일 압축해제가 안되어서 찾아보니 라이센스 동의때문에 그렇단다 그래서 위의 명령어 cookie ~~ 입력하면 된다고 하는데 해보니까 계속안되더라 그래서 찾아본 결과 sudo apt-get update, sudo apt-get install openjdk-8-jdk로 설치했다. 이걸로 시간 진짜 개잡아먹음 ㅜㅜㅜㅜ**

   > 이 설치 진행하면서 궁금한것들
   >
   > jdk jre차이
   >
   > open jdk, oracle jdk 차이
   >
   > 이렇게 또 배웁니다.. 부들부들.. 아는게너무없
   >
   > javac -version, java -version

   ```shell
   $ sudo apt-get update
   $ sudo apt-get install openjdk-8-jdk
   $ javac -version
   $ java -version
   ```

   `javac -version`과 `java -version`은 설치가됐는지 버전확인을 통해서 확인한것

   ```shell
   $ which javac
   $ readlink -f /usr/bin/javac
   $ sudo nano /etc/profile
   ```

   profile파일 가장 아래에

   `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`

   `export PATH=$JAVA_HOME/bin:$PATH`

   `export CLASS_PATH=$JAVA_HOME/lib:$CLASS_PATH`

   입력 후 ctrl + x - y - enter

   ```shell
   $ source /etc/profile
   $ echo $JAVA_HOME
   $ $JAVA_HOME/bin/javac -version
   ```

   환경변수 설정 완료

6. git 설치

   ```shell
   sudo apt-get update
   sudo apt-get install git
   git --version
   ```

   git 설치 완료

7. git clone 후 빌드

   git clone 주소

   > git clone시 github 아이디, 패스워드입력이 나오는데 매번 하기 귀찮을 수 있다. 이를 해결하기위해
   >
   > github ->  Profile -> Settings -> SSH and GPG keys ->에 공개키를 추가한다.
   >
   > 공개키는 ubuntu에서 cd ~/.ssh 후 "id_rsa.pub"라는 파일이 없다면
   >
   > ssh-keygen -t rsa로 생성한다. 뭐 확인하라고 뜨는데 걍 엔터쳐도무방
   >
   > https://proni.tistory.com/entry/%F0%9F%90%A7-Ubuntu-%EA%B3%B5%EA%B0%9C%ED%82%A4-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0참고
   >
   > 생성 후 cat ~/.ssh/id_rsa.pub 입력하면 터미널창에 나오는 값을 복붙해서 github에 등록

   ./mvnw clean package로 빌드 하면 많은 external 라이브러리를 설치

   > permission denied오류가 발생하는데 chmod +x mvnw 해주면 된다

   처음 빌드할때는 라이브러리 설치때문에 오래걸릴 수 있지만 한번 하면 그다음부터는 빠른편이다.

8. 서버 시작

   빌드가 완료되면 

   [INFO] Building jar: /home/ubuntu/SpringBoot-JPA-webBoard/target/my-slipp-1.0.jar

   라고 jar파일이 위치한 디렉토리를 볼 수 있다.

   cd target 후 java -jar my-slipp-1.0.jar해주면 서버가 실행된다 그래서 내 로컬브라우저로 접속할 수 있다.

9. 방화벽 해제

   내 로컬 브라우저로 ip로 접근하려면 해당 포트에 대한 방화벽을 해제해야한다.

   ufw명령어로 해제할 수 있다.

   기본적으로 ufw가 비활성화 상태이기 때문에 활성화한다.

   ```bash
   sudo ufw enable #활성화
   sudo ufw disable #비활성화
   sudo ufw status verbose #상태확인
   sudo ufw allow 8080/tcp # tcp, 8080포트 허용
   ```

   다시 java -jar my-slipp-1.0.jar로 서버실행 후 로컬 브라우저로 접속

   참고자료 [http://webdir.tistory/com/26](http://webdir.tistory/com/26)

   > 그래도 안되서 찾아보니까 aws ec2에 보안그룹에 사용자지정(TCP) 8080을 추가해주어야한다. 추가하니까 잘 됨!

​	웹서버 실행과 터미널 분리하려면 &를 붙여 백그라운드로 시작해도된다. 종료할때는 kill하면될듯