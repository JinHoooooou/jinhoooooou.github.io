---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(16)"
excerpt: "원격 서버 배포"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-11
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 3-5. 세 번째 반복주기 원격 서버 배포

* aws 인스턴스 콘솔 접속

* 내 IP가 바뀌었기 때문에(집에서 안하고 까페로 나옴) 보안그룹에서 인바운드 그룹의 ssh type의 ip를 수정해준다.

* key가 보관된 디렉토리로 이동해서 ssh 연결 시작

* aws 인스턴스의 연결을 눌러 복사 붙여넣기로 연결할 수도 있지만, config파일을 만들어 다음과 같이 작성해도된다.

  ```
  Host awsTemp
  	HostName 12.34.567.89
  	User	tempUser
  	indentityFile ~/tempDirectory/tempPem.pem
  ```

  이후에 git bash로 `ssh awsTemp`로 접속

* branch를 새로 만들어서 진행했기 때문에 git remote update로 원격 저장소를 update한다.
* 그리고 원격 branch 'third-cycle-studying'을 aws 인스턴스로 가져오기 위해 `git checkout -t remotes/origin/third-cycle-studying`으로 가져온다.
* 자동으로 third-cycle-studying 브랜치로 이동했기 때문에 바로 `./mvnw clean package`로 빌드한다.
* 빌드가 완료되면 target 디렉토리로 이동 후 `java -jar my-slipp.jar &`로 백그라운드로 서버를 실행한다.
* 강의를 보면 접속하면 /include/header를 로드할수없다고 에러가나오는데, 나는 된다. 근데 회원가입이나 userlist출력 버튼을 누르면 에러가 발생한다.
  
  * 유튜브 댓글들을 보니까 Controller부분에서 url처음부분의 /를 빼주면 된다고한다. 왜 그런지는 모르겠다.. 댓글들도..

