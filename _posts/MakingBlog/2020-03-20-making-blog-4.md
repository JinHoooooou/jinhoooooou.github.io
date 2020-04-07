---
title: "Github page 블로그 만들기 - 블로그에 실제 포스팅 하기"
header:
  teaser: /assets/images/2020-03-20-making-blog-4.assets/image-20200320121935474.png
excerpt: "블로그에 포스팅 하는 방법에 대해 알아보자"

categories:
 - blog
tags:
 - blog
 - github page
 - jekyll
last_modified_at: 2020-04-07
---

모든 과정은 SW developer님의 [GitHub Pages 블로그 따라하기](https://devinlife.com/howto/)를 참고하여 만들었다.

저번 포스팅에서는 지킬 테마(내 블로그 테마인 minimal mistake)를 적용해보고 내 깃헙을 이용해 호스팅까지 해보았다.

이번에는 직접 글 쓰는 방법에 대해 알아보자.

## 1. _posts 폴더에 글 쓰기

Jekyll을 이용해서 블로그를 생성했으니 정해진 포맷에 맞게 글을 등록해야한다.

폴더 구조를 보면 `_post`폴더가 있다. `_post`폴더에 md파일 확장자를 사용하며, 파일명은 yyyy-mm-dd(2020-03-19)-제목.md 포맷을 지켜야한다.

minimal-mistakes 테마는 초기에 `_post` 폴더가 없기 때문에 생성한다. 그리고 파일명 포맷에 맞춰 파일을 생성하고 글을 작성한다. (나는 Typora를 사용했다)

![image-20200320121802035]({{site.url}}/assets/images/2020-03-20-making-blog-4.assets/image-20200320121802035.png)

글을 작성 후 add, commit, push후 블로그를 확인해보자

![image-20200320121935474]({{site.url}}/assets/images/2020-03-20-making-blog-4.assets/image-20200320121935474.png)

push 후 깃헙페이지에서 자동으로 인식하여 Jekyll engine을 이용하여 html로 변환하는 작업을 수행한다. 포스팅 한 글이 업데이트 되는데 1~2분 정도 소요가 될 수 있으니 안된다고 당황해 하지 말자.

![image-20200320122149937]({{site.url}}/assets/images/2020-03-20-making-blog-4.assets/image-20200320122149937.png)

사진처럼 :heavy_check_mark: 가 보인다면 업데이트가 되었다는 뜻이다.

Jekyll이 해석하는 YFM(YAML Front Matter) 포맷에 대해 간단하게 알아보자면, YFM은 markdown 파일의 최상단에 위치하며 3개의 하이픈(-)으로 시작과 끝을 표시한다. [YAML](https://ko.wikipedia.org/wiki/YAML)은 YAML Ain't Markup Language의 약자(재귀다.)로 오픈 소스 프로젝트에서 많이 사용하는 구조화된 데이터 형식이다. YFM은 YMAL을 이용하여 글 제목, 날짜, 카테고리, 태그 등을 정의할 수 있다.

이중괄호({{}})를 이용하여 YFM에서 정의한 문구로 교체할 수 있다. 예를 들어 page.title을 이중괄호를 이용하면 {{page.title}}처럼 바꿀 수 있다. {{site.url}}이렇게

아직 YFM에 대해서 자세히 알지는 못하지만 블로그 글을 올리는 정도는 충분히 할 수 있기 때문에 이정도만 알고 넘어가고, 나중에 좀 더 알아보자.



