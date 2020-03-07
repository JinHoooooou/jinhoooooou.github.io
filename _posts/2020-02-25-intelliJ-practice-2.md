---
title: "IntelliJ 적응기(2)"
excerpt: "code style 적용"

categories:
 - Blog
tags:
 - IntelliJ
 - Java
last_modified_at: 2020-02-25
---



CheckStyle 적용

1. 플러그인 설치 : Setting - Plugins 탭 - CheckStyle 설치
2. 코드 스타일 적용 : [intellij-java-google-style.xml](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml) 설치 - Setting - Editor - Code Style탭 - Scheme 옆 설정 - import scheme - intellij idea code style xml - 설치한 xml파일 적용
3. CheckStyle 실행 : Warning항목들 대부분 whitespace나 import 패키지 정렬순서, 이름 규약 --> 수정
4. add 후 commit