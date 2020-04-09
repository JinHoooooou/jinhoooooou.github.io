---
title: "Codewars 문제풀기 (04/09)"
excerpt: "Two to One(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-09
---



## [Two to One](https://www.codewars.com/kata/5656b6906de340bd1b0000ac/train/java)

* String 2개를 인수로 받는다.
* 두 String을 합친 String을 반환한다.
* 단, 합친 String에 중복된 문자는 없어야하며, 알파벳순서로 출력된다.
* longest("aretheyhere", "yestheyarehere") --> "aehrsty"
* longest("xyaabbbccccdefww", "xxxxyyyyabklmopq") --> "abcdefklmopqwxy"
* longest("abcdefghijklmnopqrstuvwxyz", "abcdefghijklmnopqrstuvwxyz") --> "abcdefghijklmnopqrstuvwxyz"

#### 1. Test와 리팩토링

* 처음에 테스트 짜는것을 고민했다. 입력 문자열이 같을경우 그대로 입력문자열 그대로 반환하되 문자열은 알파벳 순서이고, 중복문자는 없어야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnAlphabetSortedStringWhenInputTwoStringIsSame() {
        // Given: Set input same string
        String given1 = "bacedfghikjlmnopqrstuvwxyz";
        String given2 = "bacedfghikjlmnopqrstuvwxyz";
        // When: Call twoToOne method
        String actual = TwoToOne.longest(given1, given2);
        // Then: Should return alphabet sort "abcdefghijklmnopqrstuvwxyz"
        assertEquals("abcdefghijklmnopqrstuvwxyz", actual);
      }
    ```
    
  * 실제 코드

    ```java
    import java.util.Arrays;
    
    public class TwoToOne {
    
      public static String longest(String s1, String s2) {
        String result = String.join("", s1, s2);
        char[] toCharArray = result.toCharArray();
        for (int i = 0; i < toCharArray.length; i++) {
          for (int j = 0; j < i; j++) {
            if (toCharArray[i] == toCharArray[j]) {
              toCharArray[j] = ' ';
            }
          }
        }
        Arrays.sort(toCharArray);
        return String.valueOf(toCharArray).trim();
      }
    }
    ```
    
  * 처음에 두 입력 문자열을 stream에 넣은 후 같은 문자를 삭제하는 최종연산 메서드를 사용하면 되지않을까? 생각했었는데, 그 방법을 잘 모르겠어서 char 배열로 만든 후 같은 문자가 있으면 ' '(space)로 바꾼 후 반환할때 trim()메서드로 공백을 삭제하는 방법으로 해결했다.

* 이렇게 하고 나니까 다른 모든 테스트에도 먹히는거 같은데? 라는 생각으로 문제에 있는 테스트 예제를 입력해보았다. 

  * 테스트 코드

    ```java
      @Test
      public void testShouldDeleteDuplicateCharacters() {
        // Given: Set input different string
        String given1 = "aretheyhere";
        String given2 = "yestheyarehere";
        // When: Call twoToOne method
        String actual = TwoToOne.longest(given1, given2);
        // Then: Should return alphabet sort "aehrsty"
        assertEquals("aehrsty", actual);
      }
    ```
    
  * 실제 코드 바뀐거 없이 그대로 테스트 케이스를 실행해보니 잘 돌아갔다. 그래서 일단 제출했다.

* 리팩토링

  1. join()메서드를 호출하여 지역변수에 저장할 필요없이 바로 char 배열을 사용해도 될 것 같다.

     ```java
     import java.util.Arrays;
     
     public class TwoToOne {
     
       public static String longest(String s1, String s2) {
         char[] result = String.join("", s1, s2).toCharArray();
         for (int i = 0; i < result.length; i++) {
           for (int j = 0; j < i; j++) {
             if (result[i] == result[j]) {
               result[j] = ' ';
             }
           }
         }
         Arrays.sort(result);
         return String.valueOf(result).trim();
       }
     }
     ```

     * String result 지역변수를 삭제하고, char[] 이름 toCharArray를 result로 바꿨다.
     * 2중 for문을 사용한것이 마음에 들진 않았지만 더 좋은방법이 생각이 안나서 그냥 제출했다. (리팩토링 제외 약 15분 - stream으로 해결하려고 생각하다가 방법을 몰라서 2중 for문 사용)


#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class TwoToOne {
    
    public static String longest (String s1, String s2) {
        String s = s1 + s2;
        return s.chars().distinct().sorted().collect(StringBuilder::new, StringBuilder::appendCodePoint, StringBuilder::append).toString();
    }
}
```

* distinct()가 스트림에서 중복 아이템을 제거하는 메서드라는 것을 알 수 있다.
* 그런데 뒤의 collect()메서드에서 사용되는 StringBuilder..부분을 잘 모르겠다.

* 나는 join()메서드를 사용했는데 굳이 그럴필요 없이 concat()메서드나 +연산을 사용해도 되겠구나 생각이 들었다.

그외 감명(?)받은 답변들

```java
public class TwoToOne {

    public static String longest(String s1, String s2) {
        String out = "";
        String s = s1 + s2;
        for (char c = 'a'; c <= 'z'; c++) {
            if (s.contains(c + "")) {
                out += c;
            }
        }
        return out;
    }
}
```

* 뭐 굳이 스트림 사용하지않아도 for문 하나로 처리할 수 있다.

```java
import java.util.stream.*;

public class TwoToOne {
    
    public static String longest (String s1, String s2) {
        return Stream.of(s1.concat(s2).split(""))
                  .sorted()
                  .distinct()
                  .collect(Collectors.joining());
    }
}
```

* 내가 스트림을 사용했다면 이렇게 푸는게 가장 내가 이해하기 쉬운 답변이다.



#### 3. 알게된 것

* distinct() --> 스트림의 중복 아이템들을 삭제한다.

#### 4. 모르는것

* collect의 인자로 String :: , StringBuilder::등을 사용했는데 뭔지 잘 모르겠다.