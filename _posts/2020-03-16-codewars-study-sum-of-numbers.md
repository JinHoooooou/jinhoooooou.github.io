---
title: "Codewars 문제풀기 (03/16)"
excerpt: "Sum of numbers"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-16
---



## [Sum of numbers](https://www.codewars.com/kata/55f2b110f61eb01779000053/train/java)

* 정수 두개를 인자로 받는다(a,b).
* a부터 b까지의 정수들 합을 구한다.
* a==b라면 a나 b를 리턴한다.


#### 1. Test를 만들었다

* 입력 인자가 둘 다 같을 경우

  * 테스트 코드

  ``` java
    @Test
    public void testWhenStartNumberIsSameWithEndNumber() {
      //Given
      int givenStart = 2;
      int givenEnd = 2;
      //When
      int actual = Sum.getSum(givenStart, givenEnd);
      //Then
      assertEquals(2, actual);
    }
  ```

  * 실제 코드

  ```java
  public class Sum {
  
    public static int getSum(int a, int b) {
        return a==b ? a : 0;
    }
  }
  ```

* 입력 인자 중 첫번째 인자(a)가 두번째 인자(b)보다 작은 경우

  * 테스트 코드

  ```java
    @Test
    public void testWhenStartNumberIsSmallerThanEndNumber() {
      //Given
      int givenStart = 1;
      int givenEnd = 10;
      //When
      int actual = Sum.getSum(givenStart, givenEnd);
      //Then
      assertEquals(55, actual);
    }
  ```

  * 실제 코드

  ```java
  public class Sum {
  
    public static int getSum(int a, int b) {
      if (a == b) {
        return a;
      }
      int start = 0;
      int end = 0;
      int sum = 0;
      if (a < b) {
        start = a;
        end = b;
      } 
      for (int i = start; i <= end; i++) {
        sum += i;
      }
  
      return sum;
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
  public class Sum {
  
    public static int getSum(int a, int b) {
      if (a == b) {
        return a;
      }
      int start = 0;
      int end = 0;
      int sum = 0;
      if (a < b) {
        start = a;
        end = b;
      } else {
        start = b;
        end = a;
      }
  
      for (int i = start; i <= end; i++) {
        sum += i;
      }
  
      return sum;
    }
  }
  ```

  Success (약 7분)

#### 2. 리팩토링 시작

* 대소 비교를 할 때, `if(a>b) else`를 쓰는거 보다 그냥 max, min 메소드를 써도 되겠다고 생각해서 수정

  ```java
  public class Sum {
  
    public static int getSum(int a, int b) {
        int sum = 0;
        for(int i = Math.min(a,b); i <= Math.max(a,b); i++) {
            sum += i;
        }
        return sum;
    }
  }
  ```

  이러면 a==b라도 a한번만 더하므로 분기 처리할 필요없어서 따로 추가안함

  제출

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Sum
{
  public int GetSum(int a, int b)
  {
    return (a + b) * (Math.abs(a - b) + 1) / 2;
  }
}
```

* 생각해보니 등차수열의 합이다.  첫째 항과 마지막 항, 수열 개수를 알 때 등차수열의 합은 
  * (개수 * (첫째 항 + 마지막 항)) / 2

두번째로 많이 받은 코드

```java
  public class Sum
  {
    public int GetSum(int a, int b) {
      int res = 0;
      for (int i = Math.min(a, b); i <= Math.max(a, b); i++) {
        res += i;
      }
      return a == b ? a : res;
    }
  }
```

* 내가 작성한 코드와 비슷한데 마지막에 분기처리를 해주었다.




#### 4. 궁금한거 공부

* 없음 

