---
title: "Github page 블로그 만들기 - Page 작성하기"
excerpt: "사이트 내 특정 주소에 보여줄 글인 Page를 작성해보자"
classes: wide
categories:
 - making blog
tags:
 - making blog
 - github page
 - jekyll
last_modified_at: 2020-04-07
---



모든 과정은 SW developer님의 [GitHub Pages 블로그 따라하기](https://devinlife.com/howto/)를 참고하여 만들었다.

---

지킬 블로그 기본 글 작성 타입 page글을 작성해보자. 지킬 블로그의 게시물은 post와 page로 지원된다. post는 _posts 디렉토리, page는 _pages 디렉토리에 작성한다. page는 사이트 내 특정 주소에 보여줄 글을 작성할 때(예를들면 블로그 소개 About페이지) 사용한다.



## 1. Page 글 등록하기

root 디렉토리에서 _pages 디렉토리를 생성하고 디렉토리에 md파일을 작성한다. about페이지와 404_not_found페이지르 만들어 보자.

```markdown
---
title: "About me"
permalink: /about/
layout: single
---

## About

* 광운대학교 컴퓨터정보공학부 이진호
* 백엔드 개발자가 되기위해 자바, 스프링 공부를 하고있습니다.
* 좋은 코드, 클린코드를 작성하기위해 노력하고 있습니다.
* 테스트 주도 개발(TDD)을 하기위해 노력하고 있습니다.
* 공부한 내용들을 정리해서 기록하고 수시로 remind하기위해 블로그를 만들었습니다.
* Github를 이용한 블로그 만들기, 스프링부트를 이용하여 웹 게시판 만들기 공부한 내용을 정리하여 포스팅했습니다. 
```

[about](https://jinhoooooou.github.io/about/)페이지에 대한 about.md 파일이다. YFM에서 제목, permalink, layout를 설정했다. 

* permalink는 이 페이지가 블로그 내 어느 주소에 표시될지 결정해준다. https://jinhoooooou.github.io/about/ 주소가 이 페이지의 주소이다.

* layout은 이 페이지를 어떤 형태로 보여줄지 정해준다. _layouts 디렉토리에 가면 여러가지 이름의 layout파일들을 볼 수 있다. single은 default형태의 layout이다. layout에 관한 정보는 [링크](https://mmistakes.github.io/minimal-mistakes/docs/layouts/)에서 확인할 수 있다.

## 2. 404_not_found page 등록하기

```markdown
---
title: "Page Not Found"
excerpt: "페이지를 찾을 수 없습니다."
permalink: /404.html
author_profile: false
---

요청하신 페이지를 찾을 수 없습니다.

<script>
  var GOOG_FIXURL_LANG = 'en';
  var GOOG_FIXURL_SITE = 'https://devinlife.com'
</script>
<script src="https://linkhelp.clients.google.com/tbproxy/lh/wm/fixurl.js">
</script>
```

위 내용은 404.md파일이다. Github Pages에서 404.html 페이지는 주소를 찾을 수 없을때 기본으로 보여주는 페이지이다.

블로그 내 정의되지 않은 주소로 접속을 시도하면 not found 페이지를 보여주는데 404.html 주소를 등록해두면 기본 not found 페이지가 아닌 등록한 404.html 페이지를 보여준다.

![not_found_basic]({{site.url}}/assets/images/2020-04-07-making-blog-8.assets/not_found_basic.png)

* /404.html을 설정하지 않았을 때 보여주는 Not found 페이지

![not_found_html]({{site.url}}/assets/images/2020-04-07-making-blog-8.assets/not_found_html.png)

* /404.html을 설정했을때 보여주는 Not found 페이지(author_profile : false로 했기 때문에 프로필이 보이지 않는다.)

  