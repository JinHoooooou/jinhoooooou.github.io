---
title: "Codewars 문제풀기 (03/23)"
excerpt: "너무 쉬웠던 여러 문제들"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-23
---



## 쉬웠던 여러 문제들

* 너무 쉬워서 그냥 올리지말까.. 생각하다가  올림
* 거의 한줄로 풀 수 있는.. test 생각할 필요없는 문제들
* Test를 그래도 만들어야지.. 라고 생각했는데 너무 쉽게 한줄로 나오는 문제라 그냥 품 ㅋㅋ..

### 1. Return Negative

* 양의 정수는 음의정수로, 음의정수는 그대로 반환한다.

  ```java
    public static int makeNegative(final int x) {
        return (x < 0) ? x : -x; 
    }
  ```



### 2. Sum of two lowest positive integers

* 배열에서 가장 작은 두 정수 합을 반환한다.(문제에서 Java로 못풀고 C++이나 javascript문제였는데 그냥 Java로 품)

  ```java
  public class Sum {
  
    public static int sumTwoSmallestNumbers(int[] numbers) {
  
      numbers = IntStream.of(numbers).sorted().toArray();
  
      return numbers[0] + numbers[1];
    }
  ```



### 3. Remove First and Last Character

* 문자열에서 첫문자와 마지막 문자를 삭제한 문자열을 반환한다.

  ```java
  public class RemoveChars {
      public static String remove(String str) {
          return str.substring(1, str.length() - 1);
      }
  }
  ```





