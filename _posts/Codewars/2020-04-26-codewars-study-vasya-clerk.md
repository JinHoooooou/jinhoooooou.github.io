---
title: "Codewars 문제풀기 (04/26)"
excerpt: "Vasya - Clerk(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-26
---



## [Vasya - Clerk](https://www.codewars.com/kata/555615a77ebc7c2c8a0000b8/train/java)

* int 배열을 인자로 받는다.
* 배열 element는 25, 50, 100중에 하나이며, 티켓 값은 25이다.
* 처음 거스름돈은 0원이며 배열 element에 대해 거스름돈을 줄 수 있는지 확인한다.
* 모든 element에 대해 거스름돈을 줄 수 있으면 "YES", 하나의 element라도 거스름돈 줄 잔돈이 안된다면 "NO"를 리턴한다. 
* Line.Tickets({25, 25, 50}) 👉 "YES"
* Line.Tickets({25, 25, 100}) 👉 100에 대한 거스름돈을 줄 수 없으므로 👉 "NO"
* Line.Tickets({25, 25, 50, 50, 100}) 👉 100에 대한 거스름돈을 줄 수 없으므로(25원 0개, 50원 2개) 👉 "NO" 

#### 1. Test와 리팩토링

* 테스트 1 - 입력 배열 길이가 0이라면 "NO"를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return No when input array length 0")
    public void testShouldReturnNoWhenInputArrayLength0() {
      // Given: Set line array length 0
      int[] given = new int[]{};
      // When: Call tickets method
      String actual = Line.tickets(given)
      // Then: Should return "NO"
      assertEquals("NO", actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class Line {
    
      public static String tickets(int[] peopleInLine) {
        return "NO";
      }
    }
    ```
    
  * 리팩토링 1 - NO를 상수로 추출
  
    ```java
    public class Line {
        
      static final String NO = "NO";
        
      public static String tickets(int[] peopleInLine) {
        return NO;
      }
  }
    ```
  
  * 리팩토링 2- `peopleInLine` length가 0일 때 NO리턴
  
    ```java
    public class Line {
        
      static final String NO = "NO";
        
      public static String tickets(int[] peopleInLine) {
        return peopleInLine.length == 0 ? NO : "";
      }
    }
    ```
  
  * 리팩토링 3 - 테스트 코드 When 삭제
  
    ```java
    @Test
    @DisplayName("test should return No when input array length 0")
    public void testShouldReturnNoWhenInputArrayLength0() {
      // Given: Set line array length 0
      int[] given = new int[]{};
      // Then: Should return "NO"
      assertEquals(Line.NO, line.tickets(given));
    }
    ```
  
    * When에서 메서드를 따로 호출하여 지역변수에 담아두는것보다 `assertEquals()`에 작성하는게 더 보기 좋아서 When부분을 없앴다. 그리고 "NO"부분도 Line.NO로 사용


* 테스트 2 - 배열 길이가 1이고 값이 25라면 YES 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return YES when input length 1 and element is 25")
    public void testShouldReturnYesWhenInputLength1AndValueIs25() {
      // Given: Set line array length 1 and value 25
      int[] given = new int[]{25};
      // Then: Should return "YES"
      assertEquals("YES", line.tickets(given));
    }
    ```
    
  * 실제 코드
  
    ```java
    public class Line {
        
      static final String NO = "NO";
      static final String YES = "YES";
        
      public static String tickets(int[] peopleInLine) {
        return peopleInLine.length == 0 ? NO : YES;
    }
    }
    ```
    
  * 리팩토링 - 테스트 코드 "YES"대신 Line.YES사용
  
    ```java
    @Test
    @DisplayName("test should return YES when input length 1 and element is 25")
    public void testShouldReturnYesWhenInputLength1AndValueIs25() {
      // Given: Set line array length 1 and value 25
      int[] given = new int[]{25};
      // Then: Should return "YES"
      assertEquals(Line.YES, line.tickets(given));
    }
    ```
  
    
  
* 테스트 3 - 배열 첫 value가 25가 아니라면 NO 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return NO when input length 1 and element is not 25")
    public void testShouldReturnYesWhenInputLength1AndValueIsNot25() {
      // Given: Set line array length 1 and value 50
      int[] given = new int[]{50};
      // Then: Should return "NO"
      assertEquals(Line.NO, line.tickets(given));
    }
    ```
    
  * 실제 코드

    ```java
    public class Line {
        
      static final String NO = "NO";
      static final String YES = "YES";
        
      public static String tickets(int[] peopleInLine) {
        if (peopleInLine.length == 0 || peopleInLine[0] != 25) {
          return NO;
        }
        return YES;
      }
    }
    ```

    


* 테스트 4 - 배열이 {25, 50, 50}이면 NO를 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return NO when input is {25, 50, 50}")
    public void test1() {
      // Given: Set line array {25, 25, 50}
      int[] given = new int[]{25, 50, 50};
      // Then: Should return "NO"
      assertEquals(Line.NO, line.tickets(given));
    }
    ```

  * 실제 코드

    ```java
    public class Line {
    
      static final String NO = "NO";
      static final String YES = "YES";
    
      public static String tickets(int[] peopleInLine) {
        int change25Count = 0;
        int change50Count = 0;
        int change100Count = 0;
        
        for (int i = 0; i < peopleInLine.length; i++) {
          if (peopleInLine[i] == 25) {
            change25Count++;
          } else if (peopleInLine[i] == 50) {
            if (change25Count > 0) {
              change25Count--;
              change50Count++;
            } else {
              return NO;
            } else {
              return NO;
            }
        }
        return peopleInLine.length == 0 ? NO : YES;
      }
    ```

    * 여기서 `count`들을 클래스의 필드로 빼고, for문 안의 if문들을 메서드로 추출하려고 했다. 그런데 tickets메서드가 static이기 때문에  `count`변수들도 static으로 선언하면 또 값이 변하지 않는다. 그래서 어쩔 수 없이 그냥 구현했다.

  

* 테스트 5 - 배열이 {25, 100} 이면 NO를 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return NO when input is {25, 100}")
    public void test2() {
      // Given: Set line array {25, 100}
      int[] given = new int[]{25, 100};
      // Then: Should return "NO"
      assertEquals(Line.NO, line.tickets(given));
    }
    ```

  * 실제 코드

    ```java
    package vasyaClerk_20200426;
    
    public class Line {
    
      static final String NO = "NO";
      static final String YES = "YES";
    
    
      public String tickets(int[] peopleInLine) {
        int change25Count = 0;
        int change50Count = 0;
        int change100Count = 0;
    
        for (int i = 0; i < peopleInLine.length; i++) {
          if (peopleInLine[i] == 25) {
            change25Count++;
          } else if (peopleInLine[i] == 50) {
            if (change25Count > 0) {
              change25Count--;
              change50Count++;
            } else {
              return NO;
            }
          } else if (peopleInLine[i] == 100) {
            if (change25Count > 0 && change50Count > 0) {
              change25Count--;
              change50Count--;
              change100Count++;
            } else if (change25Count > 2) {
              change25Count -= 3;
              change100Count++;
            } else {
              return NO;
            }
          } else {
            return NO;
          }
        }
        return peopleInLine.length == 0 ? NO : YES;
      }
    }
    ```

* 리팩토링 하고 싶었는데 안떠올라서 그냥 제출했다.

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Line {
    public static String Tickets(int[] peopleInLine){
        int bill25 = 0, bill50 = 0;
        for (int payment : peopleInLine){
            if(payment==25){
                bill25++;
            } else if(payment==50){
                bill25--;
                bill50++;
            } else if(payment==100){
                if(bill50>0){
                    bill50--;
                    bill25--;
                } else{
                    bill25-=3;
                }
            }
            if(bill25<0 || bill50 <0){
                return "NO";
            }
        }
        return "YES";
    }
}
```

* 더 좋은 코드가 분명 있을거라고 생각했는데, if / else if문 남발한 코드가 Best practice여서 놀랐다. 

