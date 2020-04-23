---
title: "Codewars 문제풀기 (04/23)"
excerpt: "Sum of the first nth term of Series(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-23
---



## [Sum of the first nth term of Series](https://www.codewars.com/kata/555eded1ad94b00403000071/train/java)

* int n을 인자로 받는다.
* 1 + 1/4 + 1/7 + 1/10 + 1/13 + ...을 n만큼 수행한다.
* 결과값을 소수 둘째 자리까지 반올림하여 String으로 리턴한다.
* seriesSum(0) 👉 0 👉 "0.00"
* seriesSum(1) 👉 1 👉 "1.00"
* seriesSum(2) 👉 1 + 1/4 👉 "1.25"
* seriesSum(5) 👉 1 + 1/4 + 1/7 + 1/10 + 1/13 👉 "1.57"

#### 1. Test와 리팩토링

* 테스트 1 - 입력 n이 0이라면 "0.00"을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 0.00 when input is 0")
    public void test1() {
      // Given: Set input 0
      int given = 0;
      // When: Call seriesSum method
      String actual = NthSeries.seriesSum(given);
      // Then: Should return "0.00"
      assertEquals("0.00", NthSeries.seriesSum(0));
    }
    ```
    
  * 실제 코드

    ```java
    public class NthSeries {
    
      public static String seriesSum(int n) {
        return "0.00";
      }
    }
    ```


  

* 테스트 2 - 입력 n이 1이라면 "1.00"을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 1.00 when input is 1")
    public void test2() {
      // Given: Set input 1
      int given = 1;
      // When: Call seriesSum method
      String actual = NthSeries.seriesSum(given);
      // Then: Should return "1.00"
      assertEquals("1.00", actual);
    }
    ```
    
  * 실제 코드
  
    ```java
    public class NthSeries {
    
      public static String seriesSum(int n) {
        return n==0 ? "1.00" : "0.00";
      }
    }
    ```
  
    
  
* 테스트 3 - 입력 n이 2라면 "1.25"를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 1.25 when input is 2")
    public void test3() {
      // Given: Set input 2
      int given = 2;
      // When: Call seriesSum method
      String actual = NthSeries.seriesSum(given);
      // Then: Should return "1.25"
      assertEquals("1.25", actual);
    }
    ```

  * 실제 코드

    ```java
    public class NthSeries {
    
      public static String seriesSum(int n) {
        double sum = 0;
        int series = 1;
        for (int i = 0; i < n; i++) {
          sum += (double) 1 / series;
          series += 3;
        }
    
        return String.format("%.2f", sum);
      }
    }
    ```
    
    * for문을 돌며 계속 더해주고`String.format()`으로 소수 둘째자리까지 출력하도록했다.
    
  * 리팩토링 1 - series를 안쓰고 one line으로 해결할 수 있을것 같다.

    ```java
    public class NthSeries {
    
      public static String seriesSum(int n) {
        double sum = 0;
        for (int series = 0; series < n; series++) {
          sum += (double) 1 / 1 + series * 3;
        }
    
        return String.format("%.2f", sum);
      }
    }
    ```

    

* 약 15분 소요

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class NthSeries {
  
  public static String seriesSum(int n) {
    double sum = 0.0;
    for (int i = 0; i < n; i++)
      sum += 1.0 / (1 + 3 * i);
    
    return String.format("%.2f", sum);
  }
}
```

* 똑.같.다



신박했다고 생각한 코드

```java
import java.util.stream.IntStream;

public class NthSeries {
  
  public static String seriesSum(int n) {
        return String.format("%.2f", IntStream.range(0, n).mapToDouble(num -> 1.0 / (1 + num * 3)).sum());
    }
}
```

* n~m까지의 자연수들의 합을 구할 때 for문말고 Stream을 구할 수 있을까? 생각해봤는데
* 배열이 따로 없기 때문에 배열을 선언해서 배열에 범위의 자연수들을 담고 Stream으로 구해야하나?" 라는 생각을 했었다.
* 근데 그럴거면 그냥 for문으로 구하는게 낫겠다고 생각했는데, `Instream.range()`메서드를 사용하면 되는것을 알게 되었다. 