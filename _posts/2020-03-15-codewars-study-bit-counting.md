---
title: "Codewars 문제풀기 (03/15)"
excerpt: "Bit Counting"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-15
---



## [Bit Counting](https://www.codewars.com/kata/526571aae218b8ee490006f4/train/java)

* 음의 정수가 아닌 정수를 입력받는다.
* 정수를 이진법으로 표현했을때 1인 bit 개수를 반환한다.


#### 1. Test를 만들었다

* 테스트 케이스가 정수를 입력받았을 때 1 개수를 리턴해야 하는 상황에서 경우를 더 잘게 쪼갤 수 없어서 그냥 정수 입력했을때 1 개수를 리턴하도록 단위 테스트를 만들었다.

  그리고 실제 코드를 만드려고 했는데 처음엔 인자를 0이 될때까지 계속 나누어 나머지 1의 개수를 저장해서 최종값을 반환하면 되겠다 생각했는데 Integer 클래스 메소드에 있을것 같았다. 그래서 찾아보니 `BitCounts()`메소드가 있어서 바로 써먹었다.

  ```java
  public class BitCounting {
  
    public static int countBits(int n) {
      return Integer.bitCount(n);
    }
  }
  ```

  리팩토링 할 것도 없어서 바로 제출

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class BitCounting {

  public static int countBits(int n) {
    return Integer.bitCount(n);
  }
}
```

같다.

두번째로 많이 받은 코드

```java
public class BitCounting {

  public static int countBits(int n){
    int ret = n % 2;
    while ((n /= 2) > 0) ret += n % 2;
    return ret;
  }
  
}
```

2로 계속 나누어 나머지값을 저장하였다. 처음 생각한 방법과 일치한다.

소요시간 : 약 5분


#### 4. 궁금한거 공부

* 없음 

