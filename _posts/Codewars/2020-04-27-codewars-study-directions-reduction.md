---
title: "Codewars 문제풀기 (04/27)"
excerpt: "Directions Reduction(5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-27
---



## [Directions Reduction](https://www.codewars.com/kata/550f22f4d758534c1100025a/train/java)

* String 배열을 인자로 받는다.
* 입력 배열은 "WEST", "EAST", "NORTH", "SOUTH"로 이루어져있다.
* EAST-WEST / NORTH-SOUTH 경로를 제거한 새로운 direction을 리턴한다.
* dirReduc("NORTH", "SOUTH", "SOUTH", "EAST", "WEST", "NORTH", "WEST") 👉 NORTH-SOUTH / EAST - WEST삭제 👉 "SOUTH", "NORTH", "WEST" 👉 SOUTH - NORTH 삭제 👉 "WEST" 👉 {"WEST"}
* {"NORTH", "WEST", "SOUTH", "EAST"}는 EAST - WEST / NORTH-SOUTH가 붙어있지 않기 때문에 삭제하지 않는다.

#### 1. Test와 리팩토링

* 테스트 1 - 입력 배열이 빈 배열이라면 빈 배열을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return {} when input {}")
    public void test1() {
      // Given: Set array length 0
      String[] given = new String[]{};
      // When: Call dirReduc method
      String[] actual = DirReduction.dirReduc(given);
      // Then: Should return empty array
      assertArrayEquals(given, actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class DirReduction {
      public static String[] dirReduc(String[] arr) {
        return new String[]{};
      }
    }
    ```
    
  * 리팩토링 - 테스트 코드 when 삭제
  
    ```java
    @Test
    @DisplayName("test should return {} when input {}")
    public void test1() {
      // Given: Set array length 0
      String[] given = new String[]{};
      // Then: Should return empty array
      assertArrayEquals(given, DirReduction.dirReduc(given));
    }
    ```
  
    
  


* 테스트 2 - 서로 반대되는 direction이 없을 경우 입력 배열 그대로 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return same array when there is no opposite direction")
    public void test2() {
      // Given: Set array there is no opposite direction
      String[] given = new String[]{"SOUTH", "WEST", "NORTH", "EAST"};
      // Then: Should return input array
      assertArrayEquals(given, DirReduction.dirReduc(given));
    }
    ```
    
  * 실제 코드
  
    ```java
    public class DirReduction {
      public static String[] dirReduc(String[] arr) {
        return arr;
      }
    }
    ```
    
    * 처음에 이 부분 때문에 계속 실패했다. 문제를 잘못봐서 SOUTH-WEST-NORTH-EAST로 움직이면 결국 제자리에 오니까 이것도 삭제해줘야 되는줄 알았는데 문제 테스트에 계속 실패해서 원인을 찾다보니 문제를 잘못 읽은것이었다 ㅡㅡ..
  
* 테스트 3 - 입력 배열이 {SOUTH, NORTH}라면 empty 배열을 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return {} when input array {SOUTH, NORTH}")
    public void test3() {
      // Given: Set array there is only one opposite direction
      String[] given = new String[]{"SOUTH", "NORTH"};
      // Then: Should return empty array
      assertArrayEquals(new String[]{}, DirReduction.dirReduc(given));
    }
    ```
    
  * 실제 코드

    ```java
    public class DirReduction {
      public static String[] dirReduc(String[] arr) {
        String toString = String.join(" ", arr);
        toString = toString.replace("SOUTH NORTH", "").trim();
        return toString.length()==0? new String[]{} : arr;
      }
    }
    ```
    
    * 여기서 삽질을 많이했다. if / else if문이나 switch문을 사용하려 했는데 코드가 많이 더러워질것 같아서 결국 String으로 바꾸고 replace를 이용해서 삭제하기로 했다.
  
  


* 테스트 4 - 입력 배열이 {NORTH, SOUTH, EAST}라면 {EAST} 리턴 

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return {EAST} when input array {NORTH, SOUTH, EAST}")
    public void test4() {
      // Given: Set array {"NORTH", "SOUTH", "EAST"}
      String[] given = new String[]{"NORTH", "SOUTH", "EAST"};
      // Then: Should return {"EAST"}
      assertArrayEquals(new String[]{"EAST"},DirReduction.dirReduc(given));
    }
    ```

  * 실제 코드

    ```java
    public class DirReduction {
      public static String[] dirReduc(String[] arr) {
        String toString = String.join(" ", arr);
          
        toString = toString.replace("SOUTH NORTH", "")
            .replace("NORTH SOUTH", "").trim();
          
        if (toString.length() == 0) {
          return new String[]{};
        }
        return toString.split(" ");
      }
    }
    ```
    
    * `replace()`메서드로 SOUTH NORTH, NORTH SOUTH를 삭제하면 EAST만 남기 때문에 "EAST"를 String 배열로 만들어 리턴했다.
    
  * 리팩토링 - 상수 추출

    ```java
    //실제 코드
    public static final String SOUTH_NORTH = "SOUTH NORTH";
    public static final String NORTH_SOUTH = "NORTH SOUTH";
    
    //테스트 코드
    public static final String NORTH = "NORTH";
    public static final String SOUTH = "SOUTH";
    public static final String WEST = "WEST";
    public static final String EAST = "EAST";
    ```

    

* 테스트 5 - 배열이 {NORTH, SOUTH, EAST, WEST} 이면 empty 배열을 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return {} when input array {NORTH, SOUTH, EAST, WEST}")
    public void test6() {
      // Given: Set array {NORTH, SOUTH, EAST, WEST}
      String[] given = new String[]{"NORTH", "SOUTH", "EAST", "WEST"};
      // Then: Should return empty array
      assertArrayEquals(new String[]{}, DirReduction.dirReduc(given));
    }
    ```

  * 실제 코드

    ```java
    public class DirReduction {
      
      public static final String SOUTH_NORTH = "SOUTH NORTH";
      public static final String NORTH_SOUTH = "NORTH SOUTH";
      public static final String WEST_EAST = "WEST EAST";
      public static final String EAST_WEST = "EAST WEST";
        
      public static String[] dirReduc(String[] arr) {
        String toString = String.join(" ", arr);
          
        toString = toString.replace(SOUTH_NORTH, "")
            .replace(NORTH_SOUTH, "")
            .replace(WEST_EAST, "")
            .replace(EAST_WEST, "").trim();
          
        if (toString.length() == 0) {
          return new String[]{};
        }
        return toString.split(" ");
      }
    }
    ```
    
    * 이러고 제출했는데 실패했다. 그 이유가..



* 테스트 6 - 배열이 {NORTH, EAST, WEST, SOUTH} 이면 empty 배열을 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return {} when input array {NORTH, EAST, WEST, SOUTH}")
    public void test7() {
      // Given: Set array {NORTH, EAST, WEST, SOUTH}
      String[] given = new String[]{NORTH, EAST, WEST, SOUTH};
      // Then: Should return empty array
      assertArrayEquals(new String[]{}, DirReduction.dirReduc(given));
    }
    ```

  * 실제 코드

    ```java
    public class DirReduction {
      
      public static final String SOUTH_NORTH = "SOUTH NORTH";
      public static final String NORTH_SOUTH = "NORTH SOUTH";
      public static final String WEST_EAST = "WEST EAST";
      public static final String EAST_WEST = "EAST WEST";
        
      public static String[] dirReduc(String[] arr) {
        String toString = String.join(" ", arr);
    
        toString = toString.replace(SOUTH_NORTH, "")
            .replace(NORTH_SOUTH, "")
            .replace(WEST_EAST, "")
            .replace(EAST_WEST, "")
            .replaceAll(" +", " ").trim();
    
        toString = toString.replace(SOUTH_NORTH, "")
            .replace(NORTH_SOUTH, "")
            .replace(WEST_EAST, "")
            .replace(EAST_WEST, "")
            .replaceAll(" +", " ").trim();
    
        if (toString.length() == 0) {
          return new String[]{};
        }
        return toString.split(" ");
      }
    }
    ```

    * 이전 구현을 보면 NORTH_SOUTH, SOUTH_NORTH 부터 삭제한다. 테스트 케이스를 보면 NORTH와 SOUTH는 떨어져있는데, EAST WEST를 삭제하면 다시 붙게된다. 그러면 다시 NORTH SOUTH를 삭제해야한다. 그래서 삭제하는 코드를 한번 더 붙였다.
    * 그리고 EAST WEST를 삭제하면 NORTH  SOUTH로 공백이 2번있다. 그래서 String에서 공백 2개이상을 한개로 바꿔주어야한다. 그래서 `replaceAll(" +", " ")`을 사용했다.

  * 리팩토링 - 반복문 사용

    ```java
    public class DirReduction {
      
      public static final String SOUTH_NORTH = "SOUTH NORTH";
      public static final String NORTH_SOUTH = "NORTH SOUTH";
      public static final String WEST_EAST = "WEST EAST";
      public static final String EAST_WEST = "EAST WEST";
        
      public static String[] dirReduc(String[] arr) {
        String beforeString = "";
        String afterString = String.join(" ", arr);
          
        while (beforeString.length() != afterString.length()) {
          beforeString = afterString;
          afterString = beforeString.replace(SOUTH_NORTH, "")
              .replace(NORTH_SOUTH, "")
              .replace(WEST_EAST, "")
              .replace(EAST_WEST, "")
              .replaceAll(" +", " ").trim();
        }
    
        if (afterString.length() == 0) {
          return new String[]{};
        }
        return afterString.split(" ");
      }
    }
    ```

    * 반복문을 이용하여 문자열 삭제 전과 후의 길이가 같아질 때까지 반복한다.

* if / else if문, switch문 생각 👉 `String.replace()` 생각 하는데 시간을 많이 사용했고, 공백 2개 이상을 하나로 줄이는 방법을 생각하는데 또 시간을 많이 사용했다. (약 40분)  

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Stack;

public class DirReduction {
  public static String[] dirReduc(String[] arr) {
      final Stack<String> stack = new Stack<>();

      for (final String direction : arr) {
          final String lastElement = stack.size() > 0 ? stack.lastElement() : null;

          switch(direction) {
              case "NORTH": if ("SOUTH".equals(lastElement)) { stack.pop(); } else { stack.push(direction); } break;
              case "SOUTH": if ("NORTH".equals(lastElement)) { stack.pop(); } else { stack.push(direction); } break;
              case "EAST":  if ("WEST".equals(lastElement)) { stack.pop(); } else { stack.push(direction); } break;
              case "WEST":  if ("EAST".equals(lastElement)) { stack.pop(); } else { stack.push(direction); } break;
          }
      }
      return stack.stream().toArray(String[]::new);
  }
}
```

* Best Practice를 보니 switch문을 사용하긴 했지만, 문제 의도가 stack을 이용하여 해결하는 문제라는것을 알 수 있었다.. 이런 생각을 못한게 너무 아쉽다. 흑흑

   

