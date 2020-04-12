---
title: "Codewars 문제풀기 (04/12)"
excerpt: "Sum of odd numbers(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-12
---



## [Sum of odd numbers](https://www.codewars.com/kata/55fd2d567d94ac3bc9000064/train/java)

* int를 인수로 받는다.

* ```
                             1
                           3    5
                        7    9     11
                    13    15    17    19
                 21    23    25    27    29  
  ```

* 위와 같은 형태로 홀수가 나열되어있다고 가정하자

* rowSumOddNumbers(1) --> 1 --> 1

* rowSumOddNumbers(2) --> 3 + 5 --> 8

* rowSumOddNumbers(3) --> 13 + 15 + 17 + 19 --> 27

#### 1. Test와 리팩토링

* 입력 인자가 1일경우 1을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn1WhenInputIs1() {
      // Given: Set input is 1
      int given = 1;
    
      // When: Call rowSumOddNumbers method
      int actual = RowSumOddNumbers.rowSumOddNumbers(given);
    
      // Then: Should return 1
      assertEquals(1, actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class RowSumOddNumbers {
    
      public static int rowSumOddNumbers(int n) {
        return 1;
      }
    }
    ```

* 입력 인자가 2일경우 8을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn8WhenInputIs2() {   
      // Given: Set input is 2
      int given = 2;
    
      // When: Call rowSumOddNumbers method
      int actual = RowSumOddNumbers.rowSumOddNumbers(given);
    
      // Then: Should return 8
      assertEquals(8, actual);
    }
    ```

  * 실제 코드

    ```java
    public class RowSumOddNumbers {
    
      public static int rowSumOddNumbers(int n) {
        int startIndex = 0;
        int endIndex = 0;
        for (int i = 0; i <= n; i++) {
          endIndex += i;
        }
        startIndex = endIndex - n;
        int[] oddArray = new int[endIndex];
        int odd = 1;
        for (int i = 0; i < endIndex; i++) {
          oddArray[i] = odd;
          odd += 2;
        }
        int sum = 0;
        for (int i = startIndex; i < endIndex; i++) {
          sum += oddArray[i];
        }
        return sum;
      }
    }
    ```

    * 입력 인자 n까지의 합으로 배열을 만들어 각 배열에 1, 3, 5 ...홀수들을 저장했다. 그리고  홀수 탑을 봤을 때 배열에 저장된 index가 n-1까지의 합 ~ n까지의 합이므로 startIndex와 endIndex에 각 저장 후 그 사이의 홀수들의 합을 리턴했다.

* 리팩토링을 하려했는데 작성한 테스트 코드들을 보니 1가 인자일 때 1³이고 2일 때 2³ 이다. 홀수 탑을 다시보니 3일때 3³이고 4일때 4³이다.

* 리팩토링

  * 그냥 n의 3제곱으로 해결할 수 있다.

    ```java
    public class RowSumOddNumbers {
    
      public static int rowSumOddNumbers(int n) {
        return n * n * n;
      }
    }
    ```


#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
class RowSumOddNumbers {
    public static int rowSumOddNumbers(int n) {
        return n * n * n;
    }
}
```

* 테스트 코드를 보고 문제 답의 패턴도 대충 파악할 수 있다는것을 느꼈다.

