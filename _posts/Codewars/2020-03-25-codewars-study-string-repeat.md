---
title: "Codewars 문제풀기 (03/25)"
excerpt: "String repeat"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-23
---



## [Duplicate Encoder](https://www.codewars.com/kata/54b42f9314d9229fd6000d9c/train/java)

* int와 String을 인자로 받는다.
* 인자로 받은 String을 인자로 int만큼 반복한 String을 리턴한다.
* repeatStr(5, "Hello") --> HelloHelloHelloHelloHello

#### 1. Test를 만들었다.

* String이 I, repeat수는 6

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturn6TimesIWhenInputStringIsIAndRepeat6() {
        //Given : Set string I and repeat 6
        int givenRepeat = 6;
        String givenString = "I";
        //When : Call repeatStr
        String actual = Solution.repeatStr(givenRepeat, givenString);
        //Then : Should return IIIIII
        assertEquals("IIIIII", actual);
      }
    ```
    
  * 실제 코드

    ```java
    public class Solution {
    
      public static String repeatStr(final int repeat, final String string) {
        String result = "";
        for (int i = 0; i < repeat; i++) {
          result += string;
        }
        return result;
      }
    }
    ```

* String이 Hello, repeat수는 5

  * 테스트 코드

    ```java
      @Test
      public void testShould5TimesHelloWhenInputStringIsHelloAndRepeat5() {
        //Given : Set string Hello and repeat 5
        int givenRepeat = 5;
        String givenString = "Hello";
        //When : Call repeatStr
        String actual = Solution.repeatStr(givenRepeat, givenString);
        //Then : Should HelloHelloHelloHelloHello
        assertEquals("HelloHelloHelloHelloHello", actual);
      }
    ```

  * 실제코드 그대로

  * Success(약 8분)


#### 2. 리팩토링

* 그냥 제출해도 되긴 하는데, String 가지고 연산할 때 StringBuilder쓰는 습관 들이려고 StringBuilder로 바꾸었다.

  ```java
  public class Solution {
  
    public static String repeatStr(final int repeat, final String string) {
      StringBuilder result = new StringBuilder();
      for (int i = 0; i < repeat; i++) {
        result.append(string);
      }
      return result.toString();
    }
  }
  ```

  

#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Solution {
    public static String repeatStr(final int repeat, final String string) {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < repeat; i++) {
            sb.append(string);
        }

        return sb.toString();
    }
}
```

* 같다.

* 8Kyu는 이제 쉽게 푸는거 같다. 하루에 한 문제 푸는 습관을 들이고 있는데 8Kyu가 나온 날에는 두 문제 풀어도 될것같다.



