---
title: "Codewars 문제풀기 (03/17)"
excerpt: "Persistent Bugger"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-17
---



## [Persistent Bugger](https://www.codewars.com/kata/55bf01e5a717a0d57e0000ec/train/java)

* 정수를 인자로 받는다.
* 정수를 digit단위로 쪼개서 서로 곱한다.
* 1digit가 나올때까지 반복한다.
* 곱한 횟수를 리턴한다
* persistence(39) --> 3 * 9 = 27 --> 2 * 7 =14 --> 1 * 4 = 4 --> 3리턴
* persistence(999) --> 9 * 9 * 9 = 729 --> 7 * 2 * 9 = 126 --> 1 * 2 * 6 = 12 --> 1 * 2 = 2 --> 4리턴


#### 1. Test를 만들었다

* 입력 인자가 1 digit일 경우

  * 테스트 코드

  ``` java
    @Test
    public void testWithOneDigitNumberShouldReturnZero() {
      //Given
      int given = 5;
      //When
      int actual = Persist.persistence(given);
      //Then
      assertEquals(0,actual);
    }
  ```
  
* 실제 코드
  
```java
  public class Persist {
  
    public static int persistence(long n) {
      return n < 10 ? 0 : 1;
    }
  }
```

* 입력 인자가 2 digits 이상일 경우

  * 테스트 코드

  ```java
    @Test
    public void testWithMoreThanTwoDigitNumber() {
      //Given
      int given = 786;
      //When
      int actual = Persist.persistence(given);
      //Then
      assertEquals(4,actual);
    }
  ```
  
* 실제 코드
  
```java
  public class Persist {
  
    public static int persistence(long n) {
      long result = n;
      int count = 0;
      while (result > 9) {
        String[] temp = String.valueOf(result).split("");
        result = 1;
        for (int i = 0; i < temp.length; i++) {
          result = result * Long.parseLong(temp[i]);
        }
        count++;
      }
  
      return count;
    }
  }
```

* 입력 인자 중 첫번째 인자(a)가 두번째 인자(b)보다 큰 경우

  * 테스트 코드

  ```java
    @Test
    public void testWhenStartNumberIsBiggerThanEndNumber() {
      //Given
      int givenStart = 10;
      int givenEnd = 1;
      //When
      int actual = Sum.getSum(givenStart, givenEnd);
      //Then
      assertEquals(55, actual);
    }
  ```

  * 실제 코드 

  ```java
  public class Persist {
  
    public static int persistence(long n) {
      if (n / 10 == 0) {
        return 0;
      }
  
      long result = 1;
      int count = 0;
      String[] temp = String.valueOf(n).split("");
      for (int i = 0; i < temp.length; i++) {
        result = result * Long.parseLong(temp[i]);
      }
      count++;
      while (result > 9) {
        temp = String.valueOf(result).split("");
        for (int i = 0; i < temp.length; i++) {
          result = result * Long.parseLong(temp[i]);
        }
        count++;
      }
  
      return count;
    }
  }
  ```
```
  
  Success (약 15분)

#### 2. 리팩토링 시작

* 반복문이 중복되는 코드라서 줄일 수 있을것 같아서 코드를 자세히 보니 그냥 윗부분 없애도 되겠구나 생각해서 리팩토링 함.

  ```java
  public class Persist {
  
    public static int persistence(long n) {
      long result = n;
      int count = 0;
      while (result > 9) {
        String[] temp = String.valueOf(result).split("");
        result = 1;
        for (int i = 0; i < temp.length; i++) {
          result = result * Long.parseLong(temp[i]);
        }
      count++;
      }

      return count;
    }
  }
```

  제출

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
class Persist {
  public static int persistence(long n) {
    long m = 1, r = n;

    if (r / 10 == 0)
      return 0;

    for (r = n; r != 0; r /= 10)
      m *= r % 10;

    return persistence(m) + 1;
    
  }
}
```

* 재귀함수로 풀었다. 전에도 이 문제와 비슷한 문제가 있었는데, 그 문제도 재귀함수로 푼 문제가 Best Practice를 가장 많이 받았었다.

두번째로 많이 받은 코드

```java
class Persist {
  public static int persistence(long n) {
    int times = 0;
    while (n >= 10) {
      n = Long.toString(n).chars().reduce(1, (r, i) -> r * (i - '0'));
      times++;
    }
    return times;
  }
}
```

* 스트림을 이용하여 풀었는데, reduce()와 람다식이 이해가 가질 않는다.




#### 4. 궁금한거 공부

* IntStream.reduce()

  * 스트림의 요소를 하나씩 줄여가며 누적연산을 수행한다.

  * 매개변수

    * identity : 초기값
    * accumulator : 이전 연산 결과와 스트림의 요소에 수행할 연산

    ```java
    int sum = IntStream.reduce(0, (a,b)->a+b);
    int max = IntStream.reduce(Integer.MIN_VALUE, (a,b)->a>b?a:b);
    int min = IntStream.reduce(Integer.MAX_VALUE, (a,b)->a<b?a:b);
    ```

    
