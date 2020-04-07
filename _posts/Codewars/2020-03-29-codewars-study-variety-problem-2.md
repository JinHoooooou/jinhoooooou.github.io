---
title: "Codewars 문제풀기 (03/29)"
excerpt: "매우 쉬웠던 문제들"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-26

---



## 웠던 여러 문제들



## 매우 쉬웠던 여러 문제들

* 너무 쉬워서 그냥 올리지말까.. 생각하다가  올림
* 거의 한줄로 풀 수 있는.. test 생각할 필요없는 문제들
* Test를 그래도 만들어야지.. 라고 생각했는데 너무 쉽게 한줄로 나오는 문제라 그냥 품 ㅋㅋ..

### 1. Convert a Number To a String!

* 정수를 인수로 받아 String으로 변환하여 리턴한다.

  ```java
    public static String numberToString(int num) {
        return String.valueOf(num); 
    }
  ```



### 2. Convert boolean values to Strings 'Yes' or 'No'.

* 인자가 true면 "Yes", false면 "No"를 리턴한다.

  ```java
  class YesOrNo {
    public static String YES = "Yes";
    public static String NO = "No";
  
    public static String boolToWord(boolean b) {
      return b ? YES : NO;
    } 
  }
  ```



### 3. Is this a triangle?

* 양의 정수 3개를 인수로 받아 삼각형을 만족하면 true 아니면 false를 리턴한다.

  ```java
  import java.util.Arrays;
  class TriangleTester{
    public static boolean isTriangle(int a, int b, int c){
      int[] list = {a, b, c};
      Arrays.sort(list);
      return list[0] + list[1] > list[2];
    }
  }
  ```





