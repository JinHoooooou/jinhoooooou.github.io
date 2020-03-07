---
title: "Junit Before, After"
excerpt: "setUp"

categories:
 - blog
tags:
 - blog
 - Junit
last_modified_at: 2020-02-20
---







코드리뷰 받으면서 setUp이라는것을 봤는데 Junit에서는 이것을 Before, Before class로 하는거같다

```java
    @BeforeClass
    public static void setUpBeforeClass() throws Exception {

    }

    @AfterClass
    public static void tearDownAfterClass() throws Exception {

    }

    @Before
    public void setUp() throws Exception {

    }

    @After
    public void tearDown() throws Exception {

    }
```

BeforeClass/AfterClass : 해당 테스트 메소드에서 한번만 수행되도록 지정

Before/After : 테스트 클래스에서 테스트 메소드들이 테스트되기 전/후에 실행하도록 지정

![image-20200222120558478]({{site.url}}/assets/images/2020-02-20-Junit-Before-and-After.png)



http://www.nextree.co.kr/p11104/