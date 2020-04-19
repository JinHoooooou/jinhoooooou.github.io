---
title: "Codewars 문제풀기 (04/19)"
excerpt: "Is a number prime?(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-19
---



## [Is a number prime?](https://www.codewars.com/kata/5262119038c0985a5b00029f/train/java)

* int를 인자로 받는다.
* 인자가 소수인지 소수가 아닌지 true false로 리턴한다.
* isPrime(1) 👉 false
* isPrime(-1) 👉 false
* isPrime(2) 👉 true

#### 1. Test와 리팩토링

* 테스트 1 - 인자가 음수라면 false를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldFalseWhenInputIsNegativeInteger() {
      // Given: Set negative integer
      int given = -3;
      // When: Call isPrime method
      boolean actual = Prime.isPrime(given);
      // Then: Should return false
      assertFalse(actual);
    }
    ```
    
  * 실제 코드
  
  ```java
    public class Prime {
    
      public static boolean isPrime(int num) {
        return false;
      }
    }
    ```
    
  * 리팩토링 1 - 입력 인자가 0보다 작으면 false를 리턴하도록 한다. 그리고 test코드도 Then만으로 명확히 표현할 수 있기 때문에 Given과 When을 삭제했다.

    ```java
  //테스트 코드
    @Test
    public void testShouldFalseWhenInputIsNegativeInteger() {
      // Then: Should return false
      assertFalse(Prime.isPrime(-3));
    }
    
    //실제 코드
    public class Prime {
    
      public static boolean isPrime(int num) {
        return num > 0;
      }
    }
    
    ```

  

  
* 테스트 2 - 입력 정수가 1이라면 false를 리턴한다.

  - 테스트 코드
  
    ```java
    @Test
    public void testShouldFalseWhenInputIs1() {
      // Then: Should return false
      assertFalse(Prime.isPrime(1));
    }
    ```
  
  * 실제 코드
  
    ```java
    public class Prime {
    
    public static boolean isPrime(int num) {
        return num > 1;
      }
    }
    ```



* 테스트 3 - 입력 정수가 2라면 true를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldTrueWhenInputIs2() {
      // Then: Should return true
      assertTrue(Prime.isPrime(2));
    }
    ```

  * 실제 코드 그대로



* 테스트 4 - 입력 정수가 4라면 false를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldFalseWhenInputIs4() {
      // Then: Should return false
      assertFalse(Prime.isPrime(4));
    }
    ```

  * 실제 코드

    ```java
    public class Prime {
    
      public static boolean isPrime(int num) {
        for (int i = 2; i < num; i++) {
          if (num == 2) {
            return true;
          }
          if (num % i == 0) {
            return false;
          }
        }
        return num > 1;
      }
    }
    
    ```


* 제출했는데 값이 큰 정수 (1577831221) 등은 제한시간 내 연산이 안돼서 time out오류가 났다. for문을 쓰지 않거나, 범위를 줄여야 할 것 같은데.. 고민해봐도 전혀 모르겠어서 그냥 답을 확인했다. ㅜㅜ

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.math.BigInteger;

public class Prime {

  public static boolean isPrime(int num) {
    return num > 1 && BigInteger.valueOf(num).isProbablePrime(42);
  }
}

```

* `isProbablePrime(int certainty)`메서드가 뭔지 몰라서 찾아보니까 다음과 같이 나왔다.

> ## Description
>
> The **java.math.BigInteger.isProbablePrime(int certainty)** returns true if this BigInteger is probably prime, false if it's definitely composite. If certainty is ≤ 0, true is returned.
>
> 
>
> ## Parameters
>
> **certainty** − a measure of the uncertainty that the caller is willing to tolerate: if the call returns true the probability that this BigInteger is prime exceeds (1 - 1/2certainty). The execution time of this method is proportional to the value of this parameter.

인자 certainy값은 정확도를 나타내는것 같은데.. 잘은 모르겠다. 이런 API가 있는지 몰랐는데 하나 배웠다.... 



두번째로 많이 받은 코드

```java

public class Prime {

  public static boolean isPrime(int num) {
    if (num <= 0 || num == 1) {
      return false;
    }
    int a = (int) Math.sqrt(num);

    for (int i = 2; i <= a; i++) {
      if (num % i == 0) {
        return false;
      }
    }
    return true;
  }
}
```

* for문을 사용했는데 반복 범위가 입력인자의 제곱근까지이다. 확실히 반복문 횟수를 크게 줄일 수 있는것은 알겠는데 왜 제곱근을 붙였는지 수학적으로 이해가 가질않는다 -_-;;

* 찾아보니까 

  >100의 약수는 다음과 같이 나타낼 수 있다(단, 1x100과 100x1은 제외한다).
  >2x50, 4x25, 5x20, 10x10, 20x5, 25x4, 50x2
  >
  >약수를 나타내는 방법을 살펴보면 10x10을 기준으로 대칭의 형태를 이룬다. 만약, 100이 2로 나누어떨어지지 않는다고 한다면 100이 50으로 나누어떨어지지 않는다(실제론 이렇지 않다). 따라서 특정 수(n)가 소수인지를 판별하기 위해서는 n의 제곱근 이하의 소수로만 나누어떨어지는지만 판별하면 된다.

  * 라고 하는데.. 48을 생각해보면 2 * 24, 3 * 16, 4 * 12, 6 * 8, 8 * 6, 12 * 4, 16 * 3, 24 * 2로 대칭을 이룬다 그래서 48의 제곱근인 6.xxxx의 정수형인 6까지만 체크해봐도 7부터는 대칭을 이루니까 어짜피 같기 때문에 제곱근으로 나누어 계산하는것이다.

* 수학적인 지식을 얻었다!, 왠지 나만 몰랐던 지식인듯.. ㅂㄷㅂㄷ