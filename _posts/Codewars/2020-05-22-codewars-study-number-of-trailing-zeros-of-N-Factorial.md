---
title: "Codewars 문제풀기 (05/22)"
excerpt: "Number of trailing zeros of N! (5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-22
---



# [Number of trailing zeros of N!](https://www.codewars.com/kata/52f787eb172a8b4ae1000a34/train/java)

* int n을 입력으로 받는다.

* n!에서 마지막에 붙은 0의 개수를 리턴한다.

  ``` 
  zeros(6) 👉 6! = 720 👉 1
  zeros(12) 👉 14! = 479001600 👉 2
  ```
  
  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 0일때 0을 리턴

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/c297b7a41cfdb17a5e1eba6492cfa66696678113)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/ca7b5efc8781e67808a7d883d182319907dff47f)

    ```java
    public class Solution {
    
      public static int zeros(int n) {
        return 0;
      }
    }
    ```

    

* ### 테스트 2 - 입력이 6일때 1을 리턴

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/dfef85524626afb75311ff430ae314c8e399c6f0)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/6076474afdbb8f55882e605463d43ea0e752f851)

    ```java
    public class Solution {
    
      public static int zeros(int n) {
        int fiveCount = 0;
        for (int i = 1; i <= n; i++) {
          if (i % 5 == 0) {
            fiveCount++;
          }
        }
        return fiveCount;
      }
    }
    ```
    
    * 처음에는 1부터 n까지 다 곱해준 뒤 10으로 나누어 나머지가 0이 아닐때 까지 count를 세려고 했다. 근데 문제의 hint에 "굳이 다 곱할 필요없다"라는 말을 보고 다르게 생각을 해봤는데 n!에서 0이 붙으려면 10의 몇제곱을 곱했는가를 세면된다. 그런데 1부터 n까지 연속된 수를 곱했기 때문에 2는 어짜피 많으니까 5의 개수만 세면된다. 그래서 for문을 이용해서 5의 count를 세서 리턴해주었다.
    
  
* ### 테스트 3 - 입력이 25일때 6을 리턴

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/b1d4e013df9750f19d0c638a7011e6e41e9a4ae3)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/1e22483808b18a7fc68e5628388621199ead7042)

    ```java
    public class Solution {
    
      public static int zeros(int n) {
        int fiveCount = 0;
        while (n >= 5) {
          fiveCount += n / 5;
          n /= 5;
        }
        return fiveCount;
      }
    }
    ```

    * 테스트 2의 실제 코드는 5의 거듭제곱도 5를 한번만 count한다. 그래서 반복문을 바꿨다.  n을 5로 나누었을 때 몫이 5의 개수이다. 10/5=2인데, 1~10중 5, 10으로 5가 2개있다. 25는 어떨까? 25/5=5로 1~25까지 5,10,15,20에서 5가 1개씩 있고, 25에 5가 2개 있다. 그래서 반복문을 이용했다. 


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Solution {
  public static int zeros(int n) {
    int res = 0;
    for (int i = 5; i <= n; i *= 5) {
      res += n / i;
    }
    return res;
  }
}
```

* n은 그대로 두고, 5의 거듭제곱으로 계속 나누어 몫들을 더했다. 같은 방법인데 조금 다른 느낌인데 연산 횟수는 이게 더 적은거 같다

신박하다고 생각한 코드

```java
public class Solution {
  public static int zeros(int n) {
      if(n/5 == 0)
        return 0;
      return n/5 + zeros(n/5);
  }
}
```

* 재귀로 풀었다. 
* 재귀로 풀 수 있는 문제는 재귀 생각을 못하고, 재귀로 못 푸는 문제는 재귀로 풀 수 있을거같아서 삽질하고... ㅂㄷㅂㄷ

