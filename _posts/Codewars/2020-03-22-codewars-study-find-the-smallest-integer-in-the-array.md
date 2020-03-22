---
title: "Codewars 문제풀기 (03/22)"
excerpt: "Find the smallest integer in the array"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-22
---



## [Find the smallest integer in the array](https://www.codewars.com/kata/55a2d7ebe362935a210000b2/train/java)

* int 배열을 인자로 받는다.
* 배열에서 최소값을 리턴한다.


#### 1. Test를 만들었다

* 배열 {34, 15, 88, 2}가 주어졌을 때, 2를 리턴해야한다.

  * 테스트 코드

  ``` java
    @Test
    public void testShouldReturnSmallestInteger() {
      //Given : Set integer arrays
      int[] given = {34, 15, 88, 2};
      //When : Call findSmallestInteger method
      int actual = SmallestIntegerFinder.findSmallestInt(given);
      //Then : Should return 2
      assertEquals(2,actual);
    }
  ```
  
  * 실제 코드
  
  ```java
  import java.util.Arrays;
  
  public class SmallestIntegerFinder {
  
    public static int findSmallestInt(int[] args) {
      return Arrays.stream(args).min().getAsInt();
    }
  }
  
  ```
  
  * Success(약 5분)
  



####  2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.stream.IntStream;

public class SmallestIntegerFinder {
    public static int findSmallestInt(int[] args) {
        return IntStream.of(args).min().getAsInt();
    }
}
```

* 두 문제 연속으로 너무 쉬운 문제가 나와버려서 당황스럽다.. 그래도 하루에 3문제 풀어서 다행이지 하루에 하나씩 풀었으면 김빠졌을듯..