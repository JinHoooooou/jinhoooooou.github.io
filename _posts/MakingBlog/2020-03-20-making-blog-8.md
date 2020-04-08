---
title: "Github page 블로그 만들기 - 8. Google Search Console 등록"
excerpt: "블로그에 구글 검색엔진인 Google Search Console을 등록해보자"
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

이번 포스팅에서는 구글 검색 엔진인 Google Search Console을 등록하여 내 블로그를 구글에 노출시켜보자.

## 1. Google Serach Console

Google Search Console은 구글 검색 엔진에 웹사이트가 검색되도록 등록해주고, 구글 검색 결과가 어떻게 이뤄지고 있는지 모니터링 결과도 알려준다. Google에서 웹사이트를 크롤링하여 색인 생성을 요청할 수 있고, 검색 트래픽 데이터를 확인할 수 있다. Console에 가입하지 않아도 시간이 지나면  검색결과에 포함되긴 하지만, 가입해서 색인 생성을 요청하고 웹사이트 관리를 하면 더 능동적으로 구글 검색 엔진이 웹사이트를 파악하고 개선할 수 있도록 돕는다.



## 2. Google Search Console 등록하기

1. [Google Search Console](https://search.google.com/search-console/about)사이트 접속, 구글 로그인(계정이 없다면 계정생성), 시작하기 

   ![google-console-start]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/google-console-start.png)

2. Property type 선택

   ![select-type]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/select-type.png)

   * Domain과 URL을 선택할 수 있는데, 도메인 등록 방식은 서브 도메인을 포함하는 모든 URL을 통합으로 관리하는 방식이다. 나는 Github를 통해 호스팅하였으므로 지원이 안된다.(커스텀 도메인을 등록했다면 가능하다.)
   * URL 접두어를 선택 후 URL을 입력 후 계속

3. 소유권 확인

   ![verify-ownership]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/verify-ownership.png)

   * 구글에 해당 url의 소유권자임을 증명해야한다. 구글이 권장하는 방법은 HTML file을 등록하는 것이다. 다운로드 후 내 github repository에 push한다.

   * repo에 업로드 하였다면 확인 버튼을 누른다. 바로 확인이 안된다면 몇분 기다리면 자동적으로 확인이 되어있을 것이다.

     ![verification]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/verification.png)

4. 인덱싱 요청

   ![console-main]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/console-main.png)

   * 속성으로 이동을 누르면 다음과 같은 화면이 나온다. 구글 검색을 통해 웹사이트에 유입되는 현황을 확인 할 수 있다. 그러나 내 블로그는 아직 검색되지 않았기 때문에 아무정보가 없다.

   * 구글 검색 엔진이 내 블로그를 크롤링해야 하여 어떤 정보가 있는지 분석한다. 이 과정을 인덱싱이라고 한다. 인덱싱이 완료되면 다른 사용자들이 검색창에서 내 블로그의 관련 키워드(블로그 만들기, codewars 등..)를 검색하면 결과로 내 블로그를 보여주게 된다.

   * 구글 검색 엔진에 블로그 인덱싱 요청을 보내보자.

     ![url-inspection]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/url-inspection.png)

     * 상단의 url에 있는 모든 url 검사 창에 내 블로그 기본 url을 입력하면 다음과 같은 창이 나온다.  검사 결과가 없기 때문에 색인 생성 요청을 누른다.

     * 이 방법은 일회성 요청이므로 지속적으로 google search engine이 크롤링 작업할 수 있도록 sitemap을 등록하자

       ![add-sitemap]({{site.url}}/assets/images/2020-03-20-making-blog-6.assets/add-sitemap.png)

       * `sitemap.xml`으로 작성하여 제출하면 바로 등록이 된다. 그럼 search engine이 이 파일을 기반으로 웹사이트의 업데이트된 정보를 지속적으로 크롤링 한다.

5. 갓 등록하면 데이터 처리기간이 있으므로 확인할 요소가 많이 없다. 기다리자.

