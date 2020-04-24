---
title: "Codewars 문제풀기 (04/24)"
excerpt: "Find the unique number(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-24
---



## [Find the unique number](https://www.codewars.com/kata/585d7d5adb20cf33cb000235/train/java)

* double 배열을 인자로 받는다.
* 배열의 크기는 3이상이며 배열 element는 반드시 다른 값을 가진 element가 한개 있다.
* 다른 element 값을 리턴한다.
* findUniq({1,2,1}) 👉 2
* findUniq({55,1,1,1,1,1}) 👉 55
* findUniq({0,0,0,0,0.4}) 👉 0.4

#### 1. Test와 리팩토링

* 테스트 1 - 입력 배열이 {1,2,2} 라면 1을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 1 when array is {1, 2, 2}")
    public void test1() {
      // Given: Set array first element not same
      double[] given = new double[]{2, 1, 2};
      // When: Call findUniq method
      double actual = Kata.findUniq(given);
      // Then: Should return 1
      assertEquals(1, actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class Kata {
    
      public static double findUniq(double[] arr) {
        return 1;
      }
    }
    ```
    
  * 리팩토링 - arr[0]을 리턴한다.
  
    ```java
    public class Kata {
    
      public static double findUniq(double[] arr) {
        return arr[0];
      }
    }
    ```
  
    


* 테스트 2 - 입력 배열이 {0, 0, 0, 0, 0, 0.55} 라면 0.55를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 0.55 when array is {0, 0, 0, 0, 0, 0.55}")
    public void test2() {
      // Given: Set array last element not same
      double[] given = new double[]{0, 0, 0, 0, 0, 0.55};
      // When: Call findUniq method
      double actual = Kata.findUniq(given);
      // Then: Should return 0.55
      assertEquals(0.55, actual);
    }
    ```
    
  * 실제 코드
  
    ```java
    public class Kata {
    
      public static double findUniq(double[] arr) {
        return arr[0]==arr[1] ? arr[arr.length-1] : arr[0];
      }
    }
    ```
  
    * 일단은 첫번째 element아니면 last element가 다른 값으로 가정했기 때문에 [0]==[1]이면 last element를 [0]!=[1]이라면 first element를 리턴하도록 했다.
    
    
  
* 테스트 3 - 입력 배열이 {2, 1, 2, 2, 2, 2} 라면 1을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 1 when array is {2, 1, 2, 2, 2, 2}")
    public void test3() {
      // Given: Set array middle element not same
      double[] given = new double[]{2, 1, 2, 2, 2, 2};
      // When: Call findUniq method
      double actual = Kata.findUniq(given);
      // Then: Should return 1
      assertEquals(1, actual);
    }
    ```

  * 실제 코드

    ```java
    public class Kata {
    
      public static double findUniq(double[] arr) {
        if (arr[0] != arr[1] && arr[0] != arr[2]) {
          return arr[0];
        }
        for (int i = 1; i < arr.length; i++) {
          if (arr[0] != arr[i]) {
            return arr[i];
          }
        }
        return 0;
      }
    }
    
    ```
    
    * [0]!=[1]이면 둘 중 하나를 리턴해야하는데, [0]!=[2]이면 0만 다르다는 얘기이므로 arr[0]을 리턴한다. 그 외에는 arr[0]이 답이 되는 경우가 없기 때문에 for문을 돌며 arr[0]과 다른 값을 찾아서 리턴한다.
    * 리팩토링하려고 for문을 쓰지 않고 답을 구하는 방법을 생각해봤다. 근데 떠오르지 않아서 그냥 제출했다.
    

* 약 20분 소요

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Arrays;
 public class Kata {
    public static double findUniq(double[] arr) {
      Arrays.sort(arr);
      return arr[0] == arr[1] ? arr[arr.length-1]:arr[0];
    }
}
```

* 배열을 정렬하면 값이 first element 혹은 last element 하나이다. 이 방법을 왜 생각 못했을까.... 겁나 쉽게풀었네..

