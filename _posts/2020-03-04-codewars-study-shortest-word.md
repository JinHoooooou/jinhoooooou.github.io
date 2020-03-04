---
title: "Codewars 문제풀기 (03/04)"
excerpt: "Shortest Word"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-04
---



## [Shortest Word](https://www.codewars.com/kata/57cebe1dc6fdc20c57000ac9/train/java)

* 문자열을 입력하면 문자열에서 가장 짧은 단어의 길이를 반환한다.

* findShort("my name is leejinho") --> 2

  


#### 1. Test를 만들었다

* 파라미터가 1개의 단어일때

  그냥 String의 length를 리턴하면 되겠다.

  ```java
  public static int findShort(String s) {
      return s.length();
  }
  ```

* 단어가 두개 이상일 때

  split 메소드를 사용해서 문자열 length를 비교하고 가장 짧은 길이를 리턴하면 되겠다. 

  ```java
  public static int findShort(String s) {
      String[] wordsList = s.split(" ");
      int minLengthWord = Integer.MAX_VALUE;
      for(String eachWord : wordsList) {
          minLengthWord = Math.min(minLengthWord, eachWord);
      }
      return minLengthWord;
  }
  ```
  
  * 전에 알게된 Integer.MAX_VALUE를 활용해봤다. 
  * 또한 if로 구현하지않고 바로 min메소드를 사용했다. Success. 

#### 2. 리팩토링 시작

이전에 풀었던 `highest and lowest` 문제를 풀면서 봤던 스트림, 람다로 풀어봤다. 물론 잘 모르지만 문제 자체는 거의 비슷한거같아서..

```java
public static int findShort(String s) {
    int min = Arrays.stream(s.split(" ")).mapToInt(i -> i.length()).min().getAsInt();
    return min;
}
```

Success가 뜨긴했는데 `i->i.length()`부분이 CheckStyle warning이 떠서 보니까 Integer::length나와서 바꿨다.

#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.stream.*;
public class Kata {
    public static int findShort(String s) {
        return Stream.of(s.split(" "))
          .mapToInt(String::length)
          .min()
          .getAsInt();
    }
}
```

* 똑같다.



Best Practice 두번째로 많이 받은 코드

```java
import java.util.Arrays;

public class Kata {
    public static int findShort(String s) {
        int min = Integer.MAX_VALUE;
        for(String each : s.split(" "))
        {
        if(each.length() < min)
        min = each.length();
        }
         return min;
    }
}
```

* 역시 똑같다. 이번문제는 너무 쉬었다 솔직히..


#### 5. 궁금한거 공부

* 없음