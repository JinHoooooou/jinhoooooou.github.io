---
title: "Codewars 문제풀기 (04/10)"
excerpt: "Playing with digits(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-10
---



## [Playing with digits](https://www.codewars.com/kata/5552101f47fc5178b1000050/train/java)

* int 두개를 인수로 받는다.
* 첫번째 인수 n을 digit단위로 쪼개고 각 digit를 두번째 인수 p부터 1씩 증가한값을 제곱하여 더한다.
* 계산한 값을 첫번째 인수 n으로 나누었을대 몫을 반환한다.
* 나머지가 있다면 -1을 반환한다.
* digPow(89, 1) --> 8¹ + 9² = 89 * 1 --> 1
* digPow(46288, 3) --> 4³ + 6⁴+ 2⁵ + 8⁶ + 8⁷ = 2360688 = 46288 * 51--> 51
* digPow(92, 1) --> 9¹ + 2² = 13 = 13 * k != 92 --> -1

#### 1. Test와 리팩토링

* 인수로 89, 1이 입력되면 1을 리턴해야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturn1WhenNumberIs89AndPowIs1() {
        // Given: Set number 89 and pow 1
        int givenNumber = 89;
        int givenPow = 1;
        // When: Call digPow method
        long actual = DigPow.digPow(givenNumber, givenPow);
        // Then: Should Return 1 --> 8^1 + 9^2 = 89 --> 89 = 89 * 1 --> 1
        assertEquals(1, actual);
      }
    ```
    
  * 실제 코드

    ```java
    public class DigPow {
    
      public static long digPow(int n, int p) {
        String toString = String.valueOf(n);
        String[] digits = toString.split("");
        long calResult = 0;
        for (int i = 0; i<digits.length; i++) {
          calResult += Math.pow(Integer.parseInt(digit), p);
          p++;
        }
        return calResult % n == 0 ? calResult / n : -1;
      }
    ```
    
    * n을 10으로 나눈 나머지값을 배열에 저장하는 방법도 있는데 코드가 길어질것 같아서 그냥 String으로 변환 후 split메서드로 쪼개서 배열에 저장했다. 계산할때는 parseInt 메서드를 사용해서 다시 int로 바꾸어 계산했다.
    

* 다른 테스트에도 다 먹히는것 같아서 테스트 해보니 다 통과했다.

* 리팩토링

  * 계산하는 부분을 메서드로 따로 추출했다. toString이라는 지역변수도 삭제했다.

    ```java
    public class DigPow {
    
      public static long digPow(int n, int p) {
        String[] digits = String.valueOf(n).split("");
        long calResult = calculateWithDigitsAndPow(digits, p);
    
        return calResult % n == 0 ? calResult / n : -1;
      }
    
      private static long calculateWithDigitsAndPow(String[] digits, int p) {
        long calResult = 0;
        for (String digit : digits) {
          calResult += Math.pow(Integer.parseInt(digit), p);
          p++;
        }
        return calResult;
      }
    }
    ```

* 이렇게 하고 보니까 테스트를 잘못 만든것 같다. 테스트를 그냥 예제에 맞게 테스트만 했는데.. 그래서 생각을 해봤다.

* 옛날에 처음 TDD를 강의로 배울때 계산기 기능을 TDD를 이용해서 구현하는 실습을 했었다. 그떄 생각해보면 기능(메서드)별로(+, -, *, /) 테스트를 만들었다. 테스트 메서드명도 +연산을해야한다. -연산을해야한다.. 등으로 만들었다.

* 그 기능을 고려해서 테스트 코드를 다시 짜보았다.

* 먼저 첫번째 인수를 digit단위로 쪼개는 메서드 기능을 구현해야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldSplitFirstParameterByDigit() {
        // Given: Set first parameter
        int given = 89;
        // When: Call split method
        String[] actual = DigPow.split(given);
        // Then: Should return {8, 9}
        assertArrayEquals(new int[]{8,9}, actual);
      }
    ```

  * 실제 코드

    ```java
    public class DigPow {
        
      public static String[] split(int n) {
        return String.valueOf(n).split("");
      }
    }
    
    ```

* digit단위로 쪼갠 값과 두번째 인수 pow를 이용해서 문제에서 요구하는 계산을 해야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldCalculateWithDigitAndPow() {
        // Given: Set digit array and pow
        int givenNumber = 89;
        int givenPow = 1;
        String[] givenArray = DigPow.split(givenNumber);
        // When: Call calculate method
        long actual = DigPow.calculate(givenArray, givenPow);
        // Then: Should return 89
        assertEquals(89, actual);
      }
    ```

  * 실제 코드

    ```java
    public class DigPow {
    
      public static String[] split(int n) {
    
        return String.valueOf(n).split("");
      }
    
      public static long calculate(String[] digits, int pow) {
        long result = 0;
        for (String digit : digits) {
          result += Math.pow(Integer.parseInt(digit), pow);
          pow++;
        }
        return result;
      }
    }
    ```

* calculate를 통해 얻은 값을 첫번째 인수 n으로 나눴을 때 나머지가 없다면 몫을 리턴하고 나머지가 있다면 -1을 리턴한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnQuotientWhenDivisionCalculateResultFirstParameterIsNoReminder() {
        // Given: Set number and pow
        int givenNumber = 89;
        int givenPow = 1;
        // When: Call calculate method
        long actual = DigPow.digPow(89, 1);
        // Then: Should return 1 --> 8^1 + 9^2 = 89 --> 89 = 89*1 --> 1
        assertEquals(1, actual);
      }
    ```

  * 실제 코드

    ```java
    public class DigPow {
    
      public static long digPow(int n, int p) {
        String[] digits = split(n);
        long calResult = calculateWithDigitsAndPow(digits, p);
    
        return calResult % n == 0 ? calResult / n : -1;
      }
    
      public static String[] split(int n) {
        return String.valueOf(n).split("");
      }
    
      public static long calculate(String[] digits, int pow) {
        long result = 0;
        for (String digit : digits) {
          result += Math.pow(Integer.parseInt(digit), pow);
          pow++;
        }
        return result;
      }
    }
    ```

* 리팩토링

  * split메서드는 삭제해도 될 것 같다.

    ```java
    package playingWithDigits_20200410;
    
    public class DigPow {
    
      public static long digPow(int n, int p) {
        String[] digits = String.valueOf(n).split("");
        long calResult = calculate(digits, p);
    
        return calResult % n == 0 ? calResult / n : -1;
      }
    
      public static long calculate(String[] digits, int pow) {
        long result = 0;
        for (String digit : digits) {
          result += Math.pow(Integer.parseInt(digit), pow);
          pow++;
        }
        return result;
      }
    }
    ```
    * split메서드를 삭제하면 테스트코드에서 컴파일 에러가 발생하는데.. 이럴때는 테스트 코드를 삭제해야 하는건지 아니면 테스트코드도 리팩토링해야하는지 잘 모르겠다. 


#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class DigPow {
  
  public static long digPow(int n, int p) {
    String intString = String.valueOf(n);
    long sum = 0;
    for (int i = 0; i < intString.length(); ++i, ++p)
      sum += Math.pow(Character.getNumericValue(intString.charAt(i)), p);
    return (sum % n == 0) ? sum / n : -1;
  }
  
}
```

* getNumericValue를 사용했다. 문제 푸는 방식은 비슷하다.
* 이번 문제를 풀면서 테스트 코드 작성에 좀 더 생각하는 계기가 됐다. 무작정 문제에서 제공해주는 테스트를 통과시키려고 테스트를 기계적으로 만들었는데 문제에 대한 기본 설계 후 가장 작은것을 만족하는 메서드를 먼저 만들고 점점 살을 붙여나가는 방식으로 테스트코드를 구현해야한다고 느꼈다.
* 근데 내가 한 방식이 맞는지는 잘 모르겠다. 처음 테스트 통과용으로 만든 테스트 메서드들이 뭔가 이상하다고 느껴 문제를 다 풀고나서 다시 푼것이라서 이렇게 적용시켜도 되는건지는 잘 모르겠다. 다음 이와 유사한 문제가나오면 적용해봐야 할것같다.

