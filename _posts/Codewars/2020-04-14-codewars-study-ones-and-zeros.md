---
title: "Codewars 문제풀기 (04/14)"
excerpt: "Ones And Zeros(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-14
---



## [Ones And Zeros](https://www.codewars.com/kata/578553c3a1b8d5c40300037c/train/java)

* 인자로 받은 List<Integer>의 구조는 [0, 0, 0, 0] 형태이다.
* 각 element의 값은 1 또는 0이다
* 4bit binary를 integer로 변환한 값을 리턴한다.
* [0, 0, 0, 1] --> 1
* [0, 0, 1, 1] --> 3
* [1, 0, 0, 1] --> 9

#### 1. Test와 리팩토링

* 테스트 1 - list가 [0, 0, 0, 1]일 때 --> 1 리턴

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn1WhenListArray0001() {
      // Given: Set List {0,0,0,1}
      List<Integer> given = new ArrayList<>(Arrays.asList(0,0,0,1));
      // When: Call convertBinaryToInt method
      int actual = binaryArrayToNumber.convertBinaryArrayToInt(given);
      // Then: Should return 1
      assertEquals(1, actual);
    }
    ```
    
  * 실제 코드

    ```java
    import java.util.List;
    
    public class BinaryArrayToNumber {
    
      public int convertBinaryArrayToInt(List<Integer> binary) {
        return 1;
      }
    }
    ```
  
* 테스트 2 - list가 [0, 0, 1, 0]일 때 --> 2 리턴

  - 테스트 코드

    ```java
    @Test
    public void testShouldReturn2WhenListArray0010() {
      // Given: Set List {0,0,1,0}
      List<Integer> given = new ArrayList<>(Arrays.asList(0,0,1,0));
      // When: Call convertBinaryToInt method
      int actual = binaryArrayToNumber.convertBinaryArrayToInt(given);
      // Then: Should return 2
      assertEquals(2, actual);
    }
    ```

  * 실제 코드

    ```java
    import java.util.List;
    
    public class BinaryArrayToNumber {
    
      public int convertBinaryArrayToInt(List<Integer> binary) {
        int sum = 0;
        int pow = binary.size() - 1;
        for (int i = 0; i < binary.size(); i++) {
          sum += binary.get(i) * Math.pow(2, pow);
          pow--;
        }
        return sum;
      }
    }
    ```
    
    * list의 element * 2의 제곱들을 더한 값이 결국 결과값이 된다. 제곱연산을 할 때 list의 끝(오른쪽)에서 시작하기 때문에 pow라는 지역변수를 두고 --연산을하며 계산했다.
    
  * 리팩 토링

    1. pow를 굳이 쓰지않고 바로 계산해도 될 것 같다.

       ```java
       import java.util.List;
       
       public class BinaryArrayToNumber {
       
         public int convertBinaryArrayToInt(List<Integer> binary) {
           int sum = 0;
           for (int i = 0; i < binary.size(); i++) {
             sum += binary.get(i) * Math.pow(2, binary.size() - 1 - i);
           }
           return sum;
         }
       }
       ```

    2. 개인적으로는 pow를 그냥 두는게 읽기 더 편한것 같아서 원복했다.

       ```java
       import java.util.List;
       
       public class BinaryArrayToNumber {
       
         public int convertBinaryArrayToInt(List<Integer> binary) {
           int sum = 0;
           int pow = binary.size() - 1;
           for (int i = 0; i < binary.size(); i++, pow--) {
             sum += binary.get(i) * Math.pow(2, pow);
           }
           return sum;
         }
       }
       ```

       * 제출 (약 10분 소요)



#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.List;

public class BinaryArrayToNumber {

    public static int ConvertBinaryArrayToInt(List<Integer> binary) {
        return binary.stream().reduce((x, y) -> x * 2 + y).get();
    }
}
```

* 처음에 보고 이해가 안됐다.  `reduce(x, y) -> x*2+y`는 계산한 값을 x에 담아두고, y는 element들이라고는 알고 있었는데, 왜 저게 답이 되는지 몰랐었는데 댓글을 보고 알 수 있었다.

  ```
  [a,b,c,d]
  x는 처음에 0이다.
  y는 첫번째 인덱스부터 끝까지 돈다
  (0 * 2 + a) --> x에 담아둔다
  (0 * 2 + a) * 2 + b --> x에 또 담아둔다
  ((0 * 2 + a) * 2 + b) * 2 + c --> 또 x에 담아둔다
  (((0 * 2 + a) * 2 + b) * 2 + c) * 2 + d --> list 끝까지 돌았다.
  --> (((2 * a) + b) * 2 + c) * 2 + d
  --> (2^2 * a + 2 * b + c ) * 2 + d
  --> (2^3 * a) + (2^2 * b) + (2 * c) + d
  ```

* 보고 와 신박하다.. 라고 생각이 들었다.
* 내가 직접 계산하는 불편함이 있지만 메서드명이 bit array를 integer로 변환한다는것을 명시했기 때문에 사용자 입장에서는 그냥 사용하면 되지 않을까



