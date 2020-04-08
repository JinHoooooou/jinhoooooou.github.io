---
title: "Github page 블로그 만들기 - 9. Google Analytics 등록"
excerpt: "구글 웹 분석 도구인 Google Analytics 등록해보자"
classes: wide
categories:
 - making-blog
tags:
 - making-blog
 - github-page
 - jekyll
last_modified_at: 2020-04-08
---



모든 과정은 SW developer님의 [GitHub Pages 블로그 따라하기](https://devinlife.com/howto/)를 참고하여 만들었다.

이번 포스팅에서는 구글 웹 분석 도구인 Google Analytics를 등록하여 블로그 관리를 해보자.

## 1. Google Analytics

Google Analytics는 웹 분석 도구이다. Google Search Engine과는 별개로 사람들이 본인의 블로그, 웹사이트를 어떻게 사용하는지 효과적으로 파악할 수 있게 해준다. Console에서는 블로그에 유입되는 방문자 정보를 확인할 수 있다면, Analytics는 블로그에 유입되는 모든 방문자의 정보(어떤 경로로 유입되었는지, 방문자의 위치, 네트워크 등)를 확인할 수 있다.  이런 정보를 통하여 방문자들의 현황을 파악하고 컨텐츠 구성을 개선하여 더 좋은 블로그로 만들 수 있다.



## 2. Google Analytics 등록하기

1. [Google Anlaytics](https://analytics.google.com/analytics/web/provision/#/provision)사이트 접속, 측정 시작을 눌러 계정 설정

   ![google-analytics-main]({{site.url}}/assets/images/2020-03-20-making-blog-7.assets/google-analytics-main.png)

2.  측정 시작을 누르면 계정 설정, 측정 대상, 속성 설정 창이 나온다.

   * 계정 설정

     ![create-step1]({{site.url}}/assets/images/2020-03-20-making-blog-7.assets/create-step1.png)

     * 계정 이름 설정(Email 아니어도 된다.)

   * 측정하려는 대상

     ![create-step2]({{site.url}}/assets/images/2020-03-20-making-blog-7.assets/create-step2.png)

     * 웹, app, 웹 및 앱이 있는데 나는 웹을 선택

   * 속성 설정

     ![create-step3]({{site.url}}/assets/images/2020-03-20-making-blog-7.assets/create-step3.png)

     * 웹사이트 이름과, url, 카테고리 등 설정하고 만들기 선택

3. `_config.yml`에 Tracking ID 등록

   ![tracking-id]({{site.url}}/assets/images/2020-03-20-making-blog-7.assets/tracking-id.png)

   * 만들고 나면 첫 화면에 추적 ID가 나온다. 이 ID를 `_config.yml`에 등록한다.

     ```yaml
     # Analytics
     analytics:
       provider               : google
       google:
         tracking_id          : 추적 ID
         anonymize_ip         : # true, false (default)
     ```

   * 등록 후 repository에 push한다.

4. 접속자가 늘어남에 따라 추가적으로 posting예정 ㅋㅋ..