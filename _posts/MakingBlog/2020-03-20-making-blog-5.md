---
title: "Github page 블로그 만들기 - 5. 블로그 기본 설정하기"
excerpt: "블로그 root디렉토리의 _config.yml파일을 수정하여 profile등을 수정해보자."
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

이번 포스팅에서는 최상위 폴더에 존재하는 `_config.yml`파일을 수정하여 블로그 구성을 변경하는 작업에 대해 살펴보자.

`_config.yml`파일에는 지킬 동작에 대한 설정 내용을 모두 담고 있다. 이 파일을 수정하여 블로그 구성을 설정하거나 변경할 수 있다.

간단한 포스팅의 경우는 바로 commit 후 push해서 블로그에 포스팅을 해도 되지만, 블로그 구성이나 설정 변경은 매번 push하고 확인하는 작업들이 번거롭고 불편하기 때문에, shell에서`bundle exec jekyll serve`로 확인 한 후 commit하도록하자.

참고로 `_config.yml`은 `_post`폴더의 md파일과 다르게 자동반영이 안된다. 사이트를 빌드할때 처음 `_config.yml`파일을 한번만 읽어들이기 때문에 수정한다고 자동으로 업데이트가 되지않는다. 따라서 `_config.yml`파일을 수정한 후 `bundle exec jekyll serve`로 다시 서버 실행하여 확인해야한다.

## 1. 기본 구성

![site-setting]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/site-setting-yml.png)

* minimal_mistakes_skin : 블로그 스킨을 바꿀 수 있다. 스킨 상세내용은 [link](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)참조
* locale : 지역 설정, 나는 ko-KR로 고쳤다.
* title : 사이트 상단의 제목으로 표시된다.
* name : 이름 설정
* description : 블로그 설명
  
* **여기에 작성하는 name과 description 설정은 블로그에 표시되지 않는다. **
  
* url : 페이지 url

  * 여기에 설정된 url정보는 markdown 본문에서 YFM을 이용할 때 사용된다. 

  ```yaml
  site.url + {{}} --> {{site.url}}
  ```

  `_config.yml`파일에 설정된 url정보를 기반으로 링크 주소를 구성해준다.

* teaser :  `_post`에 올린 글의 티저 사진을 등록한다. `_config.yml`에 설정된 이미지를 default로 사용하고 post마다 상단에

  ```yaml
  title : title
  header :
  	teaser : /assets/images/~~~
  ```

  로 override할 수 있다.

  ![teaser-image]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/teaser-image.png)

* logo : 상단 메뉴바에 로고를 넣을 수 있다.
  
  * **기본적으로 이미지들은 `assets/images`폴더에 저장한다.**



## 2. 오픈 그래프 이미지, 텍스트 등록

오픈그래프 프로토콜이란 SNS등에서 링크에 대한 미리보기 제목, 이미지, 설명을 구성할때 메타 데이터 표기방법이다.

![open-graph-image]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/open-graph-image.png)

* 링크로 공유할때 나오는 이미지, title, 설명등을 `_config.yml`에 등록할 수 있다.

```yaml
og_image : "/assets/images/profile.jpg"
```

블로그 home page에서는 등록된 og_image가 이미지로 나오고 post마다 override할 수 있다.

> description을 고치는 방법은 모르겠다. 이것저것 수정해봤는데 계속 바뀌질 않는다. ㅜㅜ



## 3. 블로그 주인 소개

![site-author-yml]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/site-author-yml.png)

* name, avatar, bio 등을 적어 블로그에서 자신을 소개할 수 있다.

![site-author-image]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/site-author-image.png)

## 4. 블로그 주인 소개 - Footer

![site-author-footer-yml]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/site-author-footer-yml.png)

블로그 하단의 메뉴들을 수정할 수 있다.

![site-author-footer-image]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/site-author-footer-image.png)



## 5. 블로그 표시 방법(Outputting) 설정

![outputting-yml]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/outputting-yml.png)

* paginate는 블로그 홈 페이지에서 보여주는 최근 게시물의 개수이다.
* timezone은 Asia/Seoul을 사용한다.



## 6. _posts, _pages 기본 설정

![page-post-default-image]({{site.url}}/assets/images/2020-03-20-making-blog-5.assets/page-post-default-image.png)

* 지킬에서 게시물은 크게 post와 page로 구분된다.

* 블로그 포스트들은 `_posts` 디렉토리에 위치한다. `_posts`에 속한 post들은 파일명이 yyyy-mm-dd 형식인 md파일이고 이를 지킬이 html로 변환한다.

* 블로그 글 중에서는 꼭 날짜를 기반으로 한 글만 존재하지 않는다. 블로그 주인 소개 글이나, 사이트맵 등 날짜와 관련없는 페이지들이 있을 수 있다. 이련 post들은 `_pages`디렉토리에 작성한다.

* `_config.yml`에 작성된 default 설정들은, md파일의 최상단에 작성하는 YFM을 작성하지 않아도 default로 가지는 설정들이다. 물론 YFM에 override할 수 있다. 이 방법으로 위에서 설명한 기본 teaser와 post별 teaser를 구분 하여 등록할 수 있다.

* layout이 single일 때 md파일 post에서 `classes: wide`설정을 넣어주면 더 큰 화면으로 출력된다.

  

---



### [전체적인 minimal mistakes의 설정 참고](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)

