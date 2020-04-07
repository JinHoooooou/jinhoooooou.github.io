---
title: "Github page 블로그 만들기 - 메뉴 구성하기"
excerpt: "블로그 상단의 메뉴를 구성해보자"
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

이전 포스트에서 "About" 페이지와 404페이지를 만들어서 적용해보았다. 그런데 About같은 경우는 메뉴는 따로 없이 주소창에 url을 입력하여 들어가는 방법외에 없다.

블로그 상단에 메뉴를 만들어 메뉴별로 블로그 글을 정리해보자.

## 1. 메뉴 등록하기

About메뉴를 등록해보자 메뉴 등록하기위해 _data 디렉토리에 navigation.yml 파일에 url을 작성한다.

```yaml
#main links

main:
  - title: "Home"
    url: {{site.url}}
  - title: "About"
    url: /about/
  - title: "블로그 만들기"
    url: /MakingBlog/
```

* navigation.yml 파일은 상단 메뉴바를 구성하는 설정이 있다. 메뉴 하이퍼링크 모음으로 title은 메뉴에 표시될 텍스트이고, url 항목은 메뉴가 연결될 링크 주소이다.



_pages 폴더에 makingBlog.md파일을 작성했다.

```markdown
---
title: "Github 블로그 만들기"
permalink: /makingBlog/
layout: single
author_profile: true

---



## Github Page 호스팅을 이용하여 블로그를 만들어보자

1.  [Github page 블로그 만들기 - Prologue]({{site.url}}/blog/making-blog-1/)
2.  [Github page 블로그 만들기 - Ruby와 Jekyll 설치, 기본 블로그 생성]({{site.url}}/blog/making-blog-2/)
3.  [Github page 블로그 만들기 -  Jekyll 테마선택, Github 호스팅]({{site.url}}/blog/making-blog-3/)
4.  [Github page 블로그 만들기 -  블로그에 실제 포스팅 하기]({{site.url}}/blog/making-blog-4/)
5.  [Github page 블로그 만들기 -  블로그 기본 설정하기]({{site.url}}/blog/making-blog-5/)
6.  [Github page 블로그 만들기 -  Page 작성하기]({{site.url}}/blog/making-blog-6/)
7.  [Github page 블로그 만들기 -  메뉴 구성하기]({{site.url}}/blog/making-blog-7/)
8.  [Github page 블로그 만들기 -  Google Search Console 등록]({{site.url}}/blog/making-blog-8/)
9.  [Github page 블로그 만들기 -  Google Analytics 등록]({{site.url}}/blog/making-blog-9/)


```



![menu]({{site.url}}/assets/images/2020-04-07-making-blog-7.assets/menu.png)

* 메뉴 등록이 완성되었다



## 2. 카테고리와 tags 등록하기

