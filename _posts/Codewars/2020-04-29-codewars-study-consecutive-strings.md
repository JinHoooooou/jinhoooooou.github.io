---
title: "Codewars 문제풀기 (04/29)"
excerpt: "Directions Reduction(5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-29
---



# [Consecutive strings](https://www.codewars.com/kata/56a5d994ac971f1ac500003e/train/java)

* String 배열과, int k를 인자로 받는다.
* 배열에서 k만큼 element를 합친 String중 첫번째로 가장 긴 String을 리턴한다.
* 배열 크기가 0이거나, k<=0이거나 배열 크기 < k 인경우 empty String("")를 리턴한다.

```
longestConsec({"zone", "abigail", "theta", "form", "libe", "zas", "theta", "abigail"}, 2) 👉 "abigailtheat"

longestConsec({"wlwsasphmxx","owiaxujylentrklctozmymu","wpgozvxxiu"}, 2) 👉 wlwsasphmxxowiaxujylentrklctozmymu

longestConsec({"it","wkppv","ixoyx", "3452", "zzzzzzzzzzzz"}, 3) 👉 ixoyx3452zzzzzzzzzzzz
```

## 1. Test와 리팩토링

* ### 테스트 1 - 입력 배열이 빈 배열이라면 empty String("")을 리턴

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnEmptyStringWhenInputArrayLength0() {
      // Given: Set string array length 0
      String[] givenArray = new String[]{};
      int selectCount = 0;
    
      // Then: Should return ""
      assertEquals("", LongestConsec.longestConsec(givenArray, selectCount));
    }
    ```
    
    * When없이 Then으로 처리해도 괜찮다고 생각해서 없앴다.
    
* 실제 코드
  
    ```java
    public class LongestConsec {
    
      public static String longestConsec(String[] strarr, int k) {
        return strarr.length == 0 ? "" : "no";
      }
    }
    ```
    
  

---


* ### 테스트 2 - 입력 배열 길이가 k보다 작으면 ""를 리턴

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnEmptyStringWhenInputArrayLengthIsLessThanK() {
      // Given: Set string array length less than selectCount
      String[] givenArray = new String[]{"abc", "jinho"};
      int selectCount = 5;
    
      // Then: Should return ""
      assertEquals("", LongestConsec.longestConsec(givenArray, selectCount));
    }
    ```
    
  * 실제 코드

    ```java
    public class LongestConsec {
    
      public static String longestConsec(String[] strarr, int k) {
        if (strarr.length == 0 || strarr.length < k) {
          return "";
        }
        return "no";
      }
    }
    ```

---


* ### 테스트 3 - k가 0이하면 ""를 리턴

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnEmptyStringWhenInputKIsLessThan0() {
      // Given: Set selectCount 0
      String[] givenArray = new String[]{"abc", "jinho", "sunho"};
      int selectCount = 0;
    
      // Then: Should return ""
      assertEquals("", LongestConsec.longestConsec(givenArray, selectCount));
    }
    ```
    
  * 실제 코드

    ```java
    public class LongestConsec {
    
      public static String longestConsec(String[] strarr, int k) {
        if (strarr.length == 0 || strarr.length < k || k <= 0) {
          return "";
        }
        return "no";
      }
    }
    ```

---


* ### 테스트 4 - {"zone", "abigail", "theta", "form", "libe", "zas", "theta", "abigail"} 이면 , "abigailtheta"를 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test Should return \"abigailtheta\"")
    public void testShouldReturnAbigailtheta() {
      // Given: Set array and selectCount 2
      String[] givenArray = new String[]{"zone", "abigail", "theta", "form", "libe", "zas", "theta", "abigail"};
        int selectCount = 2;
    
      // Then: Should return "abigailtheta"
      assertEquals("abigailtheta", LongestConsec.longestConsec(givenArray, selectCount));
    }
    ```

  * 실제 코드

    ```java
    public class LongestConsec {
    
      public static String longestConsec(String[] strarr, int k) {
        if (strarr.length == 0 || strarr.length < k || k <= 0) {
          return "";
        }
        
        List<String> list = new ArrayList<>();
        for (int i = 0; i < strarr.length - k + 1; i++) {
          String element = "";
          for (int j = i; j < k + i; j++) {
            element += strarr[j];
          }
          list.add(i, element);
        }
        
        String result = consecutiveStringList.get(0);
        for (String consecutiveString : consecutiveStringList) {
          if (result.length() < consecutiveString.length()) {
            result = consecutiveString;
          }
        }
        return result;
      }
    }
    ```

    * 이중 for문으로 배열의 String들을 붙여주었다.

  * 리팩토링 - 메서드로 추출

    ```java
    public class LongestConsec {
    
      public static String longestConsec(String[] strarr, int k) {
    
        if (k > strarr.length || k <= 0) {
          return "";
        }
        List<String> consecutiveStringList = makeConsecutiveString(strarr, k);
    
        String result = consecutiveStringList.get(0);
        for (String consecutiveString : consecutiveStringList) {
          if (result.length() < consecutiveString.length()) {
            result = consecutiveString;
          }
        }
        return result;
      }
    
      private static List<String> makeConsecutiveString(String[] strarr, int k) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < strarr.length - k + 1; i++) {
          String element = "";
          for (int j = i; j < k + i; j++) {
            element += strarr[j];
          }
          list.add(i, element);
        }
        return list;
      }
    }
    ```

    * 이중 for문을 돌며 consecutive string을 만드는 부분을 메서드로 추출했다.

* 다른 테스트들도 cover된다고 생각하여 제출했다(약 20분)



## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
class LongestConsec {
    public static String longestConsec(String[] strarr, int k) {
        if (strarr.length == 0 || k > strarr.length || k <= 0)
            return "";

        String longestStr = "";
        for (int index = 0; index < strarr.length - k + 1; index++) {
            StringBuilder sb = new StringBuilder();
            for (int i = index; i < index + k; i++) {
                sb.append(strarr[i]);
            }
            if (sb.toString().length() > longestStr.length()) {
                longestStr = sb.toString();
            }
        }
        return longestStr;
    }
}
```

* 리팩토링 할 부분이 더 있었는데 생각을 못했다. 굳이 List를 만들어서 consecutive string들을 저장할 필요 없이 바로 비교했어도 됐는데, 더 길게 구현했다 ㅜ
* 그리고 생각해보니까, if문으로 예외처리할 이유가 없는게, 예외 부분 케이스에서 어짜피 for문에서 걸러지기 때문에 ""가 리턴된다.

그 외 신박하다고 생각되는 코드

```java
import java.util.stream.*;
class LongestConsec {
  public static String longestConsec(String[] strarr, int k) {
    String maxStr = "";
    for (int i=0; i<=strarr.length-k; i++) {
      String current = IntStream.range(i, i+k).mapToObj(j -> strarr[j]).collect(Collectors.joining());
      if (current.length() > maxStr.length()) maxStr = current;
    }
    return maxStr;
  }
}
```

* 리팩토링 과정에서 `Stream.range()`를 이용해서 해결해도 될까? 라고 생각은 했었는데 그게 IntStream이었기 때문에 String은 안될거라고 생각했다. 그런데 `mapToObj`메서드로 구현 가능하다는것을 알게 되었다.

