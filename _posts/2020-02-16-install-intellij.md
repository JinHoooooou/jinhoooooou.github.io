---
title: "IntelliJ 학생 인증 무료 설치"
excerpt: "IntelliJ 설치"

categories:
 - blog
tags:
 - blog
 - IntelliJ
 - Java
last_modified_at: 2020-02-16
---



#### IntelliJ IDEA 학생 인증으로 무료설치

자바 개발 툴로 많이 쓰이는 이클립스(Eclipse)와 인텔리J(IntelliJ)가 있다. 이클립스는 무료이고, 인텔리J는 유료라서 나같은 학생들은 보통 이클립스를 사용하는데 인텔리J IDEA를 개발, 판매하는 업체 JetBrain에서 학생들에게 무료 라이센스를 제공하고있다.



## 설치순서

### 1. JetBrains 학생 인증 사이트

![image-20200217211131877]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217211131877.png)

[JetBarins 학생 인증 사이트](https://www.jetbrains.com/student/)로 접속하여 'Apply now'를 클릭 후 필요한 정보(이름, 국적, 학교 정보 등등)을 입력



![image-20200217211501325]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217211501325.png)

학교 메일로 확인 메일이 도착해있을것이다. 'Confirm Request'클릭 후 'I Accept'클릭



![image-20200217211634430]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217211634430.png)

'Create JetBrains Account'를 통해 계정 생성, 학교 메일로 입력해야한다.



![image-20200217211927250]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217211927250.png)

이름, username, 계정 비밀번호 입력하여 계정생성 완료



![image-20200217212129859]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217212129859.png)

계정 생성후 라이센스를 확인해보면 IntelliJ IDEA Ultimate가 있다. 들어가보자



### 2. IntelliJ 설치

IntelliJ IDEA Ultimate를 클릭하여 접속하면 바로 IntelliJ를 설치할 수 있다.

![image-20200217212255812]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217212255812.png)



![image-20200217212438120]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217212438120.png)

자신의 운영체제를 선택 후 'Ultimate download'를 클릭하면 바로 다운받을 수 있다.



![image-20200217212856421]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217212856421.png)

설치완료.



![image-20200217212941178]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217212941178.png)

처음 사용하는것이므로 아래 선택하고 OK를 눌러주자



![image-20200217213031860]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217213031860.png)

이 화면 전에 뭐 두개 뜨는데 넘어가고, UI선택화면, 당연히 Darcula사용한다 ㅎㅎ..

그 이후는 Default로 해두자, 나중에 필요할때 plugin들을 따로 설치할 수 있다고함



![image-20200217213232613]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217213232613.png)

이런 창이 나오는데 1번에서 진행했던 학생 무료 라이센스를 얻을 때 등록했던 계정입력하자



![image-20200217213416254]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217213416254.png)

이런 창이 뜨면 설치와 인증이 모두 완료된 상태이다. 'Create New Project'를 눌러 새 프로젝트 생성해보자



### 3. 프로젝트 생성

![image-20200217214128303]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217214128303.png)

음.. 뭐가 많은데 일단 Empty Project로 생성해보자



![image-20200217214202004]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217214202004.png)

프로젝트 이름은 Example로 설정



![image-20200217214321186]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217214321186.png)

프로젝트 세팅화면인데.. 일단 그냥 재끼고 OK눌러보자

![image-20200217214358363]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217214358363.png)

빈프로젝트 생성 끝



### 4. IntelliJ 기본 세팅

![image-20200217215352570]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217215352570.png)

Project의 File - Setting(Ctrl + Alt + S)



![image-20200217215635365]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217215635365.png)

Editor - General - Auto Import의 빨간 부분을 체크해주자. import가 필요한 부분이 있으면 자동으로 import 해준다.



![image-20200217215817345]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217215817345.png)

Editor - File Encodings의 인코딩을 UTF-8로 지정해준다.



![image-20200217220021104]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217220021104.png)

Keymap에서 이전까지 이클립스를 사용했으므로 이클립스 키를 사용할 수 있도록 지정해준다.



### 5. 자바 프로젝트 생성

그렇다면 자바 프로젝트 생성은 어떻게 할까

다시 New Project로 들어가보자

![image-20200217215001064]({{site.url}}/assets/images/2020-02-16-install-intellij.assets/image-20200217215001064.png)

자바 탭에 Project SDK에서 자바버전을 선택해야한다. (근데 난 자동으로 되어있었다.)

그 이후에 계속 Next누르면 그냥 자바 프로젝트가 생성된다.



---

이클립스나 sts에서 쓰던것들을 import할 수 있는지도 알아봐야겠다.



