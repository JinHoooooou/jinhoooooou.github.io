---
title: "Codewars 문제풀기 (04/09)"
excerpt: "Printer Errors(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-09
---



## [Printer Errors](https://www.codewars.com/kata/56541980fa08ab47a0000040/train/java)

* String을 인수로 받는다.
* String은 a~z문자만 있다고 가정한다.
* 입력 String에서 m-z 문자를 error문자로 보고 error 문자를 counting한다
* "(error문자수)/전체문자개수" 형태의 String을 리턴한다.

#### 1. Test와 리팩토링

* 우선 출력 포맷은 "숫자/숫자"의 형태의 String을 리턴해야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnFormattedString() {
        // Given: Set string will not print error
        String given = "abcdefghijklm";
        // When: Call printerError method
        String actual = Printer.printerError(given);
        // Then: Should Return "error/total" string format
        assertEquals("0/13", actual);
      }
    ```
    
  * 실제 코드
  
  ```java
    public class Printer {
    
      public static String printerError(String s) {
        return String.format("%d/%d", 0, 13);
      }
    }
    
    ```
    
  * 출력 포맷을 "%d/%d"형태로 하기위해 String.format()을 사용했다.
  
* 입력 String이 a~m중의 문자로만 구성되어있는 경우 즉, error 문자가 없을 경우 0/총문자개수 형태로 출력한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnNoErrorCountWhenStringContainsAtoM() {
        // Given: Set string contains a~m character
        String given = "ababcdhefefghhijkkllmmaa";
        // When: Call printerError method
        String actual = Printer.printerError(given);
        // Then: Should Return "error/total" string format
        assertEquals("0/24", actual);
      }
    ```
    
  * 실제 코드

    ```java
    public class Printer {
    
      public static String printerError(String s) {
        return String.format("%d/%d", 0, s.length());
      }
    }
    
    ```

    * error문자 개수가 없다고 가정한 테스트이기 때문에 두번째 인수를 0으로 했다.

* 입력 String이 n~z이 포함되어 있는 경우 "error문자 개수/총문자개수" 형태로 출력한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnMoreThan1ErrorCountWhenStringContainsNtoZ() {
        // Given: Set string contains a~m character
        String given = "aaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbmmmmmmmmmmmmmmmmmmmxyz";
        // When: Call printerError method
        String actual = Printer.printerError(given);
        // Then: Should Return "error/total" string format
        assertEquals("3/56", actual);
      }
    ```

  * 실제코드

    ```java
    public class Printer {
    
      public static String printerError(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
          if (s.charAt(i) > 'm') {
            count++;
          }
        }
        return String.format("%d/%d", count, s.length());
      }
    }
    ```

    * for문을 이용해서 문자열에서 'm'보다 큰 문자를 counting했다.

* 리팩토링

  1. for문 없이 stream의 filter() 메서드를 사용하면 count할 수 있을것 같다.

     ```java
     public class Printer {
     
       public static String printerError(String s) {
         long count = s.chars().filter(c -> c > 'm').count();
         return String.format("%d/%d", count, s.length());
       }
     }
     ```
     
     * format메서드 내에서 바로 호출할 수도 있지만 지역변수로 선언하는게 더 깔끔해보인다.
     
  2. 매직넘버를 상수로 추출했다.
  
     ```java
     public class Printer {
       public static final char ERROR_CHAR_INDEX = 'm';
     
       public static String printerError(String s) {
       long count = s.chars().filter(c -> c > ERROR_CHAR_INDEX).count();
         return String.format("%d/%d", count, s.length());
       }
     }
     ```
  
     * 굳이 써야하나? 라는 생각도 드는데, 배운거 써먹는거라 생각하고 리팩토링했다.
  
     * 리팩토링 요소가 더 없다고 판단해서 제출했다(리팩토링 제외 약 10분 소요)


#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Printer {
    
    public static String printerError(String s) {
        return s.replaceAll("[a-m]", "").length() + "/" + s.length();
    }
}
```

* a~m까지의 모든 문자들을 삭제하고 남은 문자열의 길이가 결국 error 문자 count가 된다. 

