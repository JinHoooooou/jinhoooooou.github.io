---
title: "Codewars 문제풀기 (03/19)"
excerpt: "Sum of positive"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-19
---



## [Sum of positive](https://www.codewars.com/kata/5715eaedb436cf5606000381/train/java)

* int형 배열을 인자로 받는다.
* 배열에서 양의 정수만 더한 값을 반환한다.
* {1, -4, 7, 12} --> 20
* 배열 인자가 없을경우 default는 0이다.


#### 1. Test를 만들었다

* 배열 요소가 없을경우 (length 0)

  * 테스트 코드

  ``` java
    @Test
    public void testWhenArrayIsNothingShouldReturnZero() {
      //Given : Set array length 0
      int[] given = {};
      //When : Call actual return value
      int actual = Positive.sum(given);
      //Then : should return 0
      assertEquals(0,actual);
    }
  
  ```
  
  * 실제 코드
  
  ```java
  public class Positive {
  
    public static int sum(int[] arr) {
      int sum = 0; 
      return sum;
    }
  }
  
  ```
  
* 배열 요소가 모두 양의 정수일 경우

  * 테스트 코드

  ```java
    @Test
    public void testWhenArrayIsComprisedWithOnlyPositiveInteger() {
      //Given : Set array to comprised with only positive integer
      int[] given = {1,2,3,4,5};
      //When : Call actual return value
      int actual = Positive.sum(given);
      //Then : should sum all element and return 15
      assertEquals(15,actual);
    }
  
  ```

  * 실제 코드

  ```java
  public class Positive {
  
    public static int sum(int[] arr) {
      int sum = 0;
      for (int i = 0; i < arr.length; i++) {
          sum += arr[i];
        }
      }
      return sum;
    }
  }
  
  ```

* 배열 요소가 양의 정수 음의 정수 모두 있을 경우

  * 테스트 코드

  ```java
    @Test
    public void testWhenArrayIsComprisedWithInteger() {
      //Given : Set array to comprised with all integer
      int[] given = {1,-2,3,4,-5};
      //When : Call actual return value
      int actual = Positive.sum(given);
      //Then : Should sum only positive integer and return 8
      assertEquals(8,actual);
    }
  ```
  
  * 실제 코드 
  
  ```java
  public class Positive {
  
    public static int sum(int[] arr) {
      int sum = 0;
      for (int i = 0; i < arr.length; i++) {
        if (arr[i] > 0) {
          sum += arr[i];
        }
      }
      return sum;
    }
  }
  
  ```
    * 배열 요소가 0 보다 클 경우만 더해주었다.
    * Success (약 10분)



#### 2. 리팩토링

`stream.filter()`를 사용하면 0보다 큰 배열 요소들만 모을 수 있으니까 한줄로 끝낼 수 있겠다.

```java
public class Positive {

  public static int sum(int[] arr) {
    return Arrays.stream(arr).filter(element -> element > 0).sum();
  }
}
```

* 여태 다른 문제들을 답들을 보고 람다식에서 i, v, s 이런식으로 표현했는데, 명확하게 변수이름을 적어주는편이 좋을거 같다 element라고 표현했다.

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Arrays;
public class Positive{
    public static int sum(int[] arr){
        return Arrays.stream(arr).filter(v -> v > 0).sum();
    }
}
```

