---
title: "Codewars 문제풀기 (04/06)"
excerpt: "Find the next perfect square!"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-30
---



## [Find the next perfect square!](https://www.codewars.com/kata/56269eb78ad2e4ced1000013/train/java)

* long을 인수로 받는다.
* long은 어떤 자연수의 제곱이며 어떤 자연수+1의 제곱을 리턴한다.
* long이 어떤 자연수의 제곱이 아니라면 -1을 리턴한다.
* findNextSquare(121) --> 11의 제곱 --> 12의 제곱인 144 리턴
* findNextSquare(625) --> 25의 제곱 --> 26의 제곱인 676 리턴
* findNextSquare(114) --> 자연수의 제곱이 아님 --> -1 리턴

#### 1. Test와 리팩토링

* 입력인수가 자연수의 제곱이 아닐경우 -1을 리턴한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnMinus1WhenInputIsNotPerfectSquare() {
        // Given: Set input integer not perfect square
        int given = 114;
        // When: Call findNextSquare method
        long actual = NumberFun.findNextSquare(given);
        // Then: Should return -1
        assertEquals(-1, actual);
      }
    ```
    
  * 실제 코드
  
    ```java
    public class NumberFun {
    
      public static long findNextSquare(long sq) {
        long temp = Math.sqrt(sq) != Math.ceil(Math.sqrt(sq)) ? -1 : 0;
        return temp;
      }
    }
    ```
    
  * 인수 sq가 자연수의 제곱인 것을 어떻게 표현할까 생각해봤는데, 예를들어 sq가 144라면 sqrt(144)는 12를 반환할 것이고, 이 값을 올림해도 12일것이다. 반면 sq가 122라면 sqrt(122)는 11.xxx를 반환할 것이고, 이 값을 올림하면 12일것이다. 따라서 `Math.sqrt(sq)!=Math.ceil(Math.sqrt(sq))` 라면 -1을 반환하도록 구현했다.
    
    * `? -1 : 0;`구절에서 바로 다음 자연수의 제곱을 리턴할 수 있지만 TDD규칙상 테스트만 만족하면 되기때문에 굳이 구현하지 않았다.
    
  * 리팩토링
  
    ```java
    public class NumberFun {
    
      public static long findNextSquare(long sq) {
        long sqrtOfSq = Math.sqrt(sq);
        return sqrtOfSq != Math.ceil(sqrtOfSq) ? -1 : 0;
      }
    }
    ```
  
    * `Math.sqrt(sq)`를 지역변수로 추출했다.
  
* 입력 인수가 625라면 26의 제곱인 676을 리턴한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturn676WhenInputIs625() {
        // Given: Set input integer 625(25 square)
        int given = 625;
        // When: Call findNextSquare method
        long actual = NumberFun.findNextSquare(given);
        // Then: Should return 676(26 square)
        assertEquals(676, actual);
      }
    ```
    
  * 실제 코드
    
    ```java
    public class NumberFun {
    
      public static long findNextSquare(long sq) {
        double sqrtOfSq = Math.sqrt(sq);
        return sqrtOfSq == Math.ceil(sqrtOfSq) ? (long) Math.pow(sqrtOfSq + 1, 2) : -1;
      }
    }
    
    ```
    
    * 이전 테스트 기준으로 작성한 코드에서 조금 바꿨다.  sq의 제곱근과 sq의 제곱근을 올림한 값이 같다면 sq의 제곱근+1 을 제곱한 값을 리턴한다.
    * 더 리팩토링할 요소가 없다고 판단해서 제출했다. (약 10분)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class NumberFun {
  public static long findNextSquare(long sq) {
      long root = (long) Math.sqrt(sq);
      return root * root == sq ? (root + 1) * (root + 1) : -1;
  }
}
```

* `Math.sqrt(sq)`의 리턴값이 double형인데 long으로 형 변환을하면 소숫점들은 버리게된다(long root). 예를들어 `Math.sqrt(sq) `가 10.6681... 이라면, long 타입으로 변환하면 그냥 10이다.
* 따라서 long타입으로 변환한 값(root)을 제곱한 값이 입력 인수와 같다면 그 값의 +1한 값의 제곱((root+1) * (root+1)) 이고 같지 않다면 -1을 리턴한다.
* 이 방법이 보기 더 직관성 있다. 그리고 `Math.pow()`메서드를 사용하지 않았는데 더 직관적인것 같다.