---
title: "Github page 블로그 만들기 - 7. 메뉴 구성하기"
excerpt: "블로그 상단의 메뉴를 구성해보자"
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

블로그 글 YMFL에 카테고리와 태그를 작성할 것이다.

```yaml
---
title: "Codewars 문제풀기"
excerpt: "Credit Card"
classes: wide
categories:
 - codewars
tags:
 - java
 - codewars
 - coding-test
last_modified_at : 2020-04-08
---
...
```

카테고리는 게시물을 제목이나 유형으로 분류할 때 사용되며, 블로그에 대한 일반적인 주제를 묶기위해 사용한다. 카테고리는 블로그가 다룰 주제들이 될 수 있다. 카테고리 구성을 잘 해야 블로그 운영하기 편하다.

태그는 해당 게시물의 세부 정보를 키워드로 설명하는 것이며, 해시태그와 유사한것으로 보면된다. 블로그에 게시물에 여러 태그를 추가할 수 있으며, 카테고리 1개, 태그 여러개 설정하는것을 추천한다.

![category-and-tag]({{site.url}}/assets/images/2020-04-07-making-blog-7.assets/category-and-tag.png)

링크로 연결될 페이지가 생성되어 있지 않기 때문에 클릭해도 Not Found를 출력할것이다.



### 카테고리, 태그 페이지,메뉴 등록

카테고리와 태그 등록을위해 _pages 디렉토리에 category-archive.md를 작성한다.

```yaml
#category-archive.md
---
title: "Post by Category"
layout: categories
permalink: /categories/
author_profile: true
---

#tags-archive.md
---
title: "Post by Tags"
layout: tags
permalink: /tags/
author_profile: true
---
```

layout이 categories인것은 모든 카테고리 별로 글을 보여주는 레이아웃 설정을 위해 사용했다. tag는 태그별로 게시물을 보여주는 레이아웃이다.

permalink는 /categories/인데 _config.yml파일에서 category_archive path 설정과 같게 설정해야한다.

```yaml
catefory_archive:
 type: liquid
 path: /categories/
tag_archive:
 type: liquid
 path: /tags/
...
```

카테고리 설정이 완료되었으면 설정한 permalink에 해당하는 url을 입력하면 Not Found가 아닌 카테고리로 모을 수 있다.

이제 _data 디렉토리의 navigation.yml에 메뉴를 등록하자

```yaml
main:
 - title: "Home"
 ...
 - title: "Categories"
   url: /categories/
 - title: "Tags"
   url: /tags/
```



![category-and-tag-menu]({{site.url}}/assets/images/2020-04-07-making-blog-7.assets/category-and-tag-menu.png)

블로그 상단의 메뉴도 올바르게 나온다. Categories와 Tags를 누르면 YFM에 작성한 category와 tag별로 post가 분류된다.



### 카테고리 분류

위에서 등록한 카테고리는 모든 카테고리들을 분류하여 출력하는 categories 레이아웃을 사용했다. 이번에는 특정지어진 1개의 카테고리를 보여주는 category 레이아웃을 사용할 것이다.

```yaml
---
title: "Github 블로그 만들기"
permalink: /categories/making-blog
layout: category
taxonomy: making-blog
---
```

permalink를 보면 categories의 하위 url이다. layout은 categories가 아니고 category이다. taxonomy가 making-blog인데 다른 post파일의 categories가 making-blog인 post만 모아서 보여준다.

![apply]({{site.url}}/assets/images/2020-04-07-making-blog-7.assets/apply.png)

 