---
title: "Codewars 문제풀기 (04/22)"
excerpt: "Are they the same(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-22
---



## [Are they the "same"?](https://www.codewars.com/kata/550498447451fbbd7600041c/train/java)

* int a b 배열 두개를 인수로 받는다.

* a배열의 원소의 제곱이 b배열 원소라면 true를 리턴하고 하나라도 아니라면 false를 리턴한다.

* a배열 b배열중 하나라도 null이라면 false를 리턴한다.

* a = [121, 144, 19, 161, 19, 144, 19, 11]  

  b = [132, 14641, 20736, 361, 25921, 361, 20736, 361]

  AreSame(a,b) --> true

* a = [121, 144, 19, 161, 19, 144, 19, 11]  

  b = [121, 14641, 20736, 36100, 25921, 361, 20736, 361]

  AreSame(a,b) --> false

#### 1. Test와 리팩토링

* 테스트 1 - 배열 b가 a의 제곱이 아니라면 false를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("배열B 원소가 배열A 원소의 제곱이 아니라면 false를 리턴한다.")
    public void testShouldFalseWhenArrayBNotHasElementOfArrayAElement() {
      // Given: A의 원소 제곱이 아닌 배열 B Set
      int[] givenA = new int[]{2, 2, 3};
      int[] givenB = new int[]{4, 9, 9};
    
      // Then: Should return false
      assertFalse(AreSame.comp(givenA, givenB));
    }
    ```
    
  * 실제 코드

    ```java
    public class AreSame {
    
      public static boolean comp(int[] a, int[] b) {
        return false;
      }
    }
    ```
  * 리팩토링 1

    ```java
    import java.util.Arrays;
    
    public class AreSame {
    
      public static boolean comp(int[] a, int[] b) {
        for (int i = 0; i < a.length; i++) {
          a[i] *= a[i];
        }
        Arrays.sort(a);
        Arrays.sort(b);
        for (int i = 0; i < a.length; i++) {
          if (a[i] != b[i]) {
            return false;
          }
        }
        return true;
      }
    }
    ```
    * 배열 a의 원소들을 제곱하여 다시 저장하고, 배열을 정렬하여 a와 b를 비교하며 같지 않으면 false를 리턴하도록 했다.

  * 리팩토링 2 - `Arrays.eqauls()`사용

    ```java
    import java.util.Arrays;
    
    public class AreSame {
    
      public static boolean comp(int[] a, int[] b) {
        for (int i = 0; i < a.length; i++) {
          a[i] *= a[i];
        }
        Arrays.sort(a);
        Arrays.sort(b);
        return Arrays.equals(a,b);
      }
    }
    ```

  * 리팩토링 3 - for문 대신 Stream 사용

    ```java
    import java.util.Arrays;
    
    public class AreSame {
    
      public static boolean comp(int[] a, int[] b) {
    	a = Arrays.stream(a).map(x->x*x).sorted().toArray();
        Arrays.sort(b);
        return Arrays.equals(a,b);
      }
    }
    ```

    

* 테스트 2 - 배열 b가 a의 제곱이라면 true를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("배열B 원소가 배열A 원소의 제곱이 맞다면 true를 리턴한다.")
    public void testShouldTrueWhenArrayBIsSquareOfArrayAElement() {
      // Given: A의 원소 제곱 배열 B Set
      int[] givenA = new int[]{121, 144, 19, 161, 19, 144, 19, 11};
      int[] givenB = new int[]{121, 14641, 20736, 36100, 25921, 361, 20736, 361};
    
      // Then: Should return true
      assertFalse(AreSame.comp(givenA, givenB));
    }
    ```
    
  * 실제 코드 그대로
  
* 테스트 3 - 배열 a나 b가 null이라면 false를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("배열B나 배열A가 null이라면 false를 리턴한다.")
    public void testShouldfalseWhenArrayBOrArrayAIsNull() {
      // Given: A배열을 null로 set
      int[] givenA = null;
      int[] givenB = new int[]{121, 14641, 20736, 36100, 25921, 361, 20736, 361};
    
      // Then: Should return false
      assertFalse(AreSame.comp(givenA, givenB));
    }
    ```

  * 실제 코드

    ```java
    import java.util.Arrays;
    
    public static boolean comp(int[] a, int[] b) {
      if (a == null || b == null) {
        return false;
      }
      a = Arrays.stream(a).map(x -> x * x).sorted().toArray();
      Arrays.sort(b);
      return Arrays.equals(a, b);
    }
    ```

* 처음에 배열 a의 원소들의 제곱이 배열 b에 포함되어있으면 true를 리턴하는것으로 이해해서 이중 for문으로 원소를 하나씩 비교하면서 해결하려 했다. 그런데 문제를 다시 보니까 배열 길이가 a, b배열이 같아서 포함이 아니라 원소가 같으면(순서에 상관없이) true를 리턴하는구나 알게되어서 간단하게 해결했다.(약 30분 소요)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.*;
import java.io.*;
import java.util.Arrays;

public class AreSame {
  
  public static boolean comp(int[] a, int[] b) {
    if ((a == null) || (b == null)){
          return false;
    }
    int[] aa = Arrays.stream(a).map(n -> n * n).toArray();
    Arrays.sort(aa);
    Arrays.sort(b);
    return (Arrays.equals(aa, b));
    
  }
}
```

* aa를 따로 저장한거 외에는 다른게 없다.
