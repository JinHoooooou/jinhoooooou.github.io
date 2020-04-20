---
title: "Codewars 문제풀기 (04/20)"
excerpt: "Build a pile of Cubes(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-20
---



## [Build a pile of Cubes](https://www.codewars.com/kata/5592e3bd57b64d00f3000047/train/java)

* long을 인자로 받는다.
* 1³ + 2³ + 3³ ... + (n-1)³ + n³ = 인자로 받은 수가 되는 자연수 n을 리턴한다.
* n이 없다면 -1을 리턴한다.

#### 1. Test와 리팩토링

* 테스트 1 - 인자가 1이라면 1을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn1WhenInput1() {
      // Given: Set long 1
      long given = 1;
      // When: Call findNb method
      long actual = ASum.findNb(given);
      // Then: Should return 1
      assertEquals(1, actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class ASum {
    
      public static long findNb(long m) {
        return 1;
      }
    }
    ```

* 테스트 2 - 인자가 9라면 2를 리턴한다. (1³ + 2³ = 9)

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn2WhenInput9() {
      // Given: Set long 9
      long given = 9;
      // When: Call findNb method
      long actual = ASum.findNb(given);
      // Then: Should return 2
      assertEquals(2, actual);
    }
    ```

  * 실제 코드

    ```java
    public class ASum {
    
      public static long findNb(long m) {
        long sum = 0;
        for (long i = 0; sum <= m; i++) {
          sum += (i * i * i);
          if (sum == m) {
            return i;
          }
        }
        return 0;
      }
    }
    ```

    * sum을 선언하여 for문에서 1^3부터 계속 더해준다. sum이 인자와 같아지면 i를 리턴한다.

* 테스트 3 - 인자가 8이라면 -1을 리턴해야한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnNegative1WhenInput8() {
      // Given: Set long 8
      long given = 8;
      // When: Call findNb method
      long actual = ASum.findNb(given);
      // Then: Should return -1
      assertEquals(-1, actual);
    }
    ```

  * 실제 코드

    ```java
    public class ASum {
    
      public static long findNb(long m) {
        long sum = 0;
        for (long i = 0; sum <= m; i++) {
          sum += (i * i * i);
          if (sum == m) {
            return i;
          }
        }
        return -1;
      }
    }
    ```

    * 반복문을 나가면 0대신 -1을 리턴하도록 변경

  * 리팩토링 - for문 대신 while문 사용

    ```java
    public class ASum {
    
      public static long findNb(long m) {
        long sum = 0;
        long result = 0;
        while (sum < m) {
          result++;
          sum += (result * result * result);
        }
        return sum == m ? result : -1;
      }
    }
    ```

* 고등학교 때 배운 1~n까지 합(∑k), 1² ~ n²까지 합(∑k²) 등이 생각 났는데 식 자체는 몰라서 그냥 제출했다.(약 10분)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class ASum {
  
  public static long findNb(long m) {
    long mm = 0, n = 0;
    while (mm < m) mm += ++n * n * n;
    return mm == m ? n : -1;
  } 
}
```

* 나는 `result++; sum += ....`으로 풀었는데 그냥 `sum += ++result * ...`으로 했으면 한줄로 해결할 수 있다.

