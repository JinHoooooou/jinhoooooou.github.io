---
title: "간단한 카카오 API 이용 - 1. Setting"
header:
  teaser: /assets/images/header/teaser-image/lion.png
excerpt: "카카오 API를 이용해서 나에게 메시지를 보내보자"
classes: wide

categories:
 - 카카오-API
tags:
 - kakao-api
last_modified_at: 2020-10-29
---


## 재밌겠다

* 예전에 Slack에서 내가 채팅을 치면 채팅 메시지에 따라 내가 원하는 정보를 출력하도록 Slack bot을 만들어 본 적이 있다.(매우 간단하게..) 비슷하게 카카오톡에서도 가능할까? 생각했었는데 카카오 developers에서 api를 제공해줘서 할 수 있을거 같다는 생각이 들어서 시작했다.

------

### 1. 카카오 developers

* [카카오 developers](https://developers.kakao.com) - 회원가입 or 로그인

### 2. 내 애플리케이션 - 애플리케이션 추가하기

* ![my-application]({{site.url}}/assets/images/Kakao-Message-API/my-application.png)
  * 앱 아이콘, 앱 이름, 사업자명 입력

### 3. 앱키 확인

* ![app-key]({{site.url}}/assets/images/Kakao-Message-API/app-key.png)
  * REST API 키 확인

### 4. 플랫폼 등록

* 왼쪽의 플랫폼 메뉴

* Web 등록
  * 사이트 도메인 등록하여 메시지의 링크에 해당하는 도메인주소로 갈 수 있도록 설정
    * ex) https://naver.com 등록을 하지 않으면 메시지링크에 네이버가 있어도 가질못한다.
* 등록 후 redirect url도 등록
  * redirect url은 카카오 로그인 access code 확인 위해 필요함

### 5. 카카오 로그인

* 왼쪽의 카카오 로그인 메뉴

* 활성화

  ![kakao-login]({{site.url}}/assets/images/Kakao-Message-API/kakao-login.png)

### 6.동의 항목

* 왼쪽의 동의항목 메뉴

* '프로필 정보(닉네임/프로필사진)' 설정 클릭 후 동의, 동의, 목적 작성

  ![profile]({{site.url}}/assets/images/Kakao-Message-API/profile.png)

* 접근권한 관리의 '카카오톡 메시지 전송' 설정 클릭 후 동의, 목적 작성

  ![kakao-message-send]({{site.url}}/assets/images/Kakao-Message-API/kakao-message-send.png)



---



기본세팅 완료. 다음엔 카카오 로그인 API를 이용해서 ouath token을 받는 작업 할 것이다.



