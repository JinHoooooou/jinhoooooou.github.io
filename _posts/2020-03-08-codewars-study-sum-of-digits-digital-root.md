---
title: "Codewars 문제풀기 (03/08)"
excerpt: "Sum of Digits / Digital Root"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-08
---



## [Sum of Digits / Digital Root](https://www.codewars.com/kata/541c8630095125aba6000c00/train/java)

* 자연수 n을 인자로 받는다.

* n의 각 digit를 더한다.

* 더한 값이 2 digit 이상이라면 다시 각 digit을 더한다

* 최종 합이 1 digit이 될때 까지 반복하고 최종 1 digit을 반환한다.

* digital_root(16) --> 1 + 6 --> 7

* digital_root(942) --> 9 + 4 + 2 --> 15 --> 1 + 5 --> 6

* digital_root(132189) --> 1 + 3 + 2 + 1 + 8 + 9 --> 24 --> 2 + 4 --> 6

  


#### 1. Test를 만들었다

* 파라미터가 1digit일때

  그냥 입력한 자연수 n을 리턴하면되겠다.

  ```java
  public static int digital_root(int n) {
      return n;
  }
  ```

* 파라미터가 2digit 이상일 때

  각 digit를 더하는 부분을 메소드로 추출해서 결과가 1 digit가 나올때까지 반복하면되겠다.

  ```java
  public static int digital_root(int n) {
      int result = n;
      while (result > 9) {
        result = splitDigitAndAddEachDigit(result);
      }
      return result;
    }
  
    private static int splitDigitAndAddEachDigit(int n) {
      int sum = 0;
      while (n > 0) {
        sum += n % 10;
        n /= 10;
      }
      return sum;
    }
  ```
  
  * Success, 리팩토링 할게 더 없어 보여서 그냥 제출했다.

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public static int digital_root(int n) {
    return (n != 0 && n%9 == 0) ? 9 : n % 9;
}
```

* 갑자기 한줄로 답이나와서 놀랬는데 직접 계산해보니 모든 답들은 9의 나머지로 나왔다. 이런답을 보면 항상 신기하다..



내 답과 가장 비슷한 코드

```java
public static int digital_root(int n) {   
    if (n < 10)
      return n;
      
    int dr = 0; 
    while (n >= 1) {
      dr += n % 10;
      n = n / 10;
    }
    
    return digital_root(dr);
}
```

* 다 같은데 이 답은 재귀함수를 이용해서 풀었다. 이 방법도 괜찮은것 같다.


#### 3. 궁금한거 공부

* 없음