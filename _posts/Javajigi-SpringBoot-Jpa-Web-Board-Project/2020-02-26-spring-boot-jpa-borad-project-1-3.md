---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(3)"
excerpt: "local 소스코드를 github에 올리기"

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

### 1-3. local 소스코드를 github에 올리기

github에 소스코드 추가

sourcetree활용 -> 나는 그냥 cli로

1. 원격 레포에 모든 파일을 업로드할 필요는 없다. 보통 코드들은 개발자들이 수정하기때문에 업로드하지만 메이븐같은 빌드도구에 의해 자동으로 생성되는 코드들도있다. 이런코드들은 굳이 깃헙에 올릴필요가없다.

2. 따라서 gitignore로 업로드목록에서 제거할수있다. .으로 시작하는 파일/디렉토리들은 깃헙을 통해 관리할 필요가없다. ignore해보자

   > .ignore파일에 무시할 파일목록들이 적혀있다. vi .gitignore를 보면 STS, intelliJ, VScode등 IDEA별로 잘 정리되어 있다. 추가할 파일목록을 적어주면된다.
   >
   > 하나 팁 : *.file 하면 확장자명이 file인 모든 파일을 무시한다. 그런데 모든 file확장자를 무시하되 한 program.file은 추가해야한다면 !program.file를 따로 써주면된다.

3. git add , push로 원격 레포에 소스코드를 업로드

   > git add . 하니까 모든 파일에대해서 'LF will be replaced by CRLF'라는 warning 메세지가 뜬다. 이유를 찾아보니까 맥,리눅스와 / 윈도우 환경에서 whitespace에 대한 에러라고한다. 유닉스 시스템은 한줄의 끝이 LF(line feed)로 이루어지고, 윈도우에서는 줄 하나가 CR(Carriage Return)과 LF(Line Feed)로 이루어지기 때문이라고한다. 그래서 git이 어느것을 선택할지 몰라 그런 warning메세지를 계속 보냈던것
   >
   > core.autocrlf라는 기능을 켜주면 해결해준다고한다.
   >
   > `git config --global core.autocrlf true`명령어를 입력하면 완료

