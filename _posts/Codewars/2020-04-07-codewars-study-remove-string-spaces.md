---
title: "Codewars 문제풀기 (04/07)"
excerpt: "Remove String Spaces"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-07
---



## [Remove String Spaces](https://www.codewars.com/kata/57eae20f5500ad98e50002c5/train/java)

* String을 인수로 받는다
* String에 있는 space를 제거한 string을 리턴한다.
* noSpace("8 j 8   mBliB8g  imjB8B8  jl  B") --> "8j8mBliB8gimjB8B8jlB"

#### 1. Test와 리팩토링

* 입력 String 인수에 space가 없다면 입력 인수를 그대로 리턴한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnOriginalStringWhenInputHasNoSpace() {
        // Given: Set string no has space
        String given = "mynameisleejinho";
        // When: Call noSpace method
        String actual = Kata.noSpace(given);
        // Then: Should return given string
        assertEquals(given, actual);
      }
    ```
    
  * 실제 코드

    ```java
    public class Kata {
    
      public static String noSpace(final String x) {
        return x.split(" ").length == 1 ? x : "";
      }
    }
    ```
    
  * space기준으로 split한 문자열들이 length가 1이라면 space가 없다는 의미이다. 따라서 공백이 없으면 입력 string 그대로 리턴하도록 구현했다.

    * 첫번째 테스트를 만족하는 코드만 작성했다.

* 입력 String 인수에 space가 있고, 연속된 space가 없을 경우 

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnNoSpaceStringWhenInputHasSpaceButNoConsecutiveSpace() {
        // Given: Set string only one space, not consecutive space
        String given = "my name is lee jin ho";
        // When: Call noSpace method
        String actual = Kata.noSpace(given);
        // Then: Should return given string
        assertEquals("mynameisleejinho", actual);
      }
    ```
    
  * 실제 코드

    ```java
    import java.util.Arrays;
    import java.util.stream.Collectors;
    
    public class Kata {
    
      public static String noSpace(final String x) {
        String result = Arrays.stream(x.split(" ")).collect(Collectors.joining());
        return x.split(" ").length == 1 ? x : result;
      }
    }
    ```

    * space를 기준으로 split한 문자열들을 stream에 담고  `Collectotrs.joining()`으로 문자열들을 합치는 방법으로 구현했다.

  * 리팩토링

    ```java
    import java.util.Arrays;
    import java.util.stream.Collectors;
    
    public class Kata {
      public static String noSpace(final String x) {
        return Arrays.stream(x.split(" ")).collect(Collectors.joining());
      }
    }
    ```

    * split한 문자열들의 개수를 파악할 필요 없이 바로 join을 해도 되기때문에 삭제했다.
    * 더 리팩토링할 요소가 없어보여서 그대로 제출했다(약 15분 소요)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
class Kata {
    static String noSpace(final String x) {
        return x.replace(" ", "");
    }
}
```

* `replace()` 메서드는 첫번째 인자로 들어가는 문자열을 두번째 인자로 들어가는 문자열로 치환하는 함수이다. 그래서 space(" ")를 ""로 치환해서 공백을 없앤다.
* 이 방법이 내가 stream사용해서 코드를 주렁주렁 늘리는것보다 훨씬 간편하다.
* 요즘 문제를 풀 때 stream을 이용하자? 라는 강박관념이 조금 잡혀있는것같다. 더 간단하게 풀 수 있는방법을 생각해보고 풀어야겠다.



#### 3. 모르는것 공부

* `replace()`와 `replaceAll()`차이

  * replace는 인자로 `char`이나 `charSequence`가 들어간다. 따라서 해당 문자열만 치환한다.

  * replaceAll은 인자로 regax가 들어간다. 따라서 해당 문자열 match가 되면 치환한다.

  * ```java
    String ex = "aaabbbccccabcddddabcdeeee";
    String replace = ex.replace("abc", "틀");
    String replaceAll = ex.replaceAll("[abc]", "틀");
    
    replace : aaabbbcccc틀dddd틀deeee
    replaceAll : 틀틀틀틀틀틀틀틀틀틀틀틀틀dddd틀틀틀deeee
    ```



