---
title: "Codewars 문제풀기 (03/03)"
excerpt: "Square Every Digit"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-03
---



## [SquareEveryDigit](https://www.codewars.com/kata/546e2562b03326a88e000020/train/java)

* integer를 입력하면 해당 integer의 digit들의 2제곱을 integer로 출력

* squareDigits(9119) --> 811181

  


#### 1. Test를 만들었다

* 숫자가 하나일 때 

  입력한 숫자의 제곱을 반환하면 되겠구나

  ```java
  public int squareDigits(int n) {
      return n * n;
  }
  ```

* 숫자가 두개 이상일 때

  문자열 digit나눠서 해당 integer의 제곱을 string에 붙이자 

  ```java
  public int squareDigit(int n) {
      List<Integer> list = new ArrayList<>();
      while (n > 0) {
        int digit = n % 10;
        list.add(0, digit * digit);
        n /= 10;
      }
      StringBuilder result = new StringBuilder();
      for (int i = 0; i<list.size(); i++) {
        result.append(list.get(i));
      }
      return Integer.parseInt(result.toString());
  }
  ```
  
  Success. 

#### 2. 리팩토링 시작

for문대신 for each문 쓰는게 더 좋을거같아서 수정

```java
public int squareDigit(int n) {
    List<Integer> list = new ArrayList<>();
    while (n > 0) {
      int digit = n % 10;
      list.add(0, digit * digit);
      n /= 10;
    }
    StringBuilder result = new StringBuilder();
    for (Integer integer : list) {
      result.appned(integer);
    }
    return Integer.parseInt(result.toString());
}
```



#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class SquareDigit {

  public int squareDigits(int n) {
    String result = ""; 
    
    while (n != 0) {
      int digit = n % 10 ;
      result = digit*digit + result ;
      n /= 10 ;
    }
    
    return Integer.parseInt(result) ;
  }

}

```

* 굳이 List를 쓰지않아도 되겠구나라고 느낌 logic 자체는 비슷한거같다.


#### 4. 궁금한거 공부

* 없음