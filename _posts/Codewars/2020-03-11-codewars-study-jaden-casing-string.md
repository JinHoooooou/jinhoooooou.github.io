---
title: "Codewars 문제풀기 (03/11)"
excerpt: "Jaden Casing Strings"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-10
---



## [Jaden Casing Strings](https://www.codewars.com/kata/5390bac347d09b7da40006f6/train/java)

* 문자열을 인자로 받는다.
* space(" ")를 기준으로 첫 글자만 대문자로 바꿔 출력한다.
* 인자가 "", null인경우 null을 반환한다.


#### 1. Test를 만들었다

* 문자열이 null이거나 ""일때

  null이거나 length가 0일때 null 반환

  ```java
  public class JadenCase {
    public static String toJadenCase(String phrase) {
      if (phrase == null || phrase.length() == 0) {
        return null;
      }
        return "";
    }
  }
  
  ```

* 그 외의 경우

  `String.split()`으로 단어별로 쪼갠후 첫 글자만 대문자로 변환

  ```java
  package jadenCase;
  public class JadenCase {
    public static String toJadenCase(String phrase) {
      if (phrase == null || phrase.length() == 0) {
        return null;
      }
      String[] words = phrase.split(" ");
      for (int i = 0; i < words.length; i++) {
        words[i] = words[i].substring(0, 1).toUpperCase() + words[i].substring(1);
      }
  
      return String.join(" ", words);
    }
  }
  ```
  
  저번에 알게된 `String.join()`을 써먹었다.


#### 2. 리팩토링 시작

계속해서 Stream을 사용해서 코드를 줄이려는 노력을 하고 있어서 Stream으로 해결하려고했다.

```java
package jadenCase;
public class JadenCase {
  public static String toJadenCase(String phrase) {
      return Arrays.stream(phrase.split(" "))
        .map(s -> s.substring(0, 1).toUpperCase() + s.substring(1)).collect(
            Collectors.joining(" "));
  }
}
```

* 보통 스트림생성().중개연산().최종연산(); 형태를 가지고 있기 때문에 먼저 `Arrays.stream(phrase.split(" "))`으로 stream을 생성하고 이전까지의 문제 경험상 map()을 많이 썼기 때문에 `map()`으로 중개연산을 진행했다. 그리고 지난 문제들에서 `joining()`이 Collectors를 이용한것을 봤었기 때문에 이를 이용했다.

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.lang.Character;

public class JadenCase {

  public String toJadenCase(String phrase) {
    if(phrase == null || phrase.equals("")) return null;
    
    char[] array = phrase.toCharArray();
    
    for(int x = 0; x < array.length; x++) {
      if(x == 0 || array[x-1] == ' ') {
        array[x] = Character.toUpperCase(array[x]);
      }
    }
    
    return new String(array);
  }

}
```

* Charcter의 toUpperCase를 이용했다. if문 안의 내용은 첫글자와 해당 배열의 이전 값이 ' '(space)라면 대문자로 바꿔주는 뜻이다. 

두번째로 많이 받은 코드

```java
import java.util.Arrays;
import java.util.stream.Collectors;

public class JadenCase {

  public String toJadenCase(String phrase) {
      if (null == phrase || phrase.length() == 0) {
          return null;
      }

      return Arrays.stream(phrase.split(" "))
                   .map(i -> i.substring(0, 1).toUpperCase() + i.substring(1, i.length()))
                   .collect(Collectors.joining(" "));
  }

}
```

* 스트림을 이용했고, 내 답이랑 똑같다. 처음으로 스트림을 이용해서 잘 해결한 문제라 기분좋다(?)


#### 4. 궁금한거 공부

* 없음 