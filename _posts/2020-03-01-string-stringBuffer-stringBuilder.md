---
title: "String, StrnigBuffer, StringBuilder 차이"
excerpt: ""

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-01
---



## String, StringBuffer, StringBuilder

* String과 StringBuilder, StringBuffer의 차이

  * String은 불변객체이므로 새로운 값을 할당할때마다 새로 생성된다. String 클래스가 생성될때 내부 변수와 메서드들도 같이 생성되며 각 String의 주소값이 stack에 쌓이고 클래스들은 garbage collector가 호출되기전까지 heap에 쌓이므로 많이 사용될수록 메모리 관리측면에서 치명적이다.

  * StringBuilder와 StringBuffer는 객체 생성을 한번만 하고 append를 이용해 메모리 값을 변경시켜 문자열을 변경한다. 

  * 따라서 String은 문자열을 단순히 읽는 작업을 할때 좋고, StringBuilder와 StringBuffer는 문자열 연산이 자주있을때 사용하면 좋다.

* StringBuilder와 StringBuffer의 차이 

  * StringBuffer는 멀티 쓰레드 환경에서 동기화가 가능하지만 StringBuilder는 동기화를 지원하지 않는다.
  * 따라서 싱글 쓰레드환경에서는 StringBuilder가 적합하고, 멀티 쓰레드 환경에서는 StringBuffer가 적합하다.

---

* 참고자료
  * [https://novemberde.github.io/2017/04/15/String_0.html](https://novemberde.github.io/2017/04/15/String_0.html)
  * [https://jeong-pro.tistory.com/85](https://jeong-pro.tistory.com/85)



* 후에 추가적으로 공부할것
  * Thread
  * garbage collector
  * stack, heap