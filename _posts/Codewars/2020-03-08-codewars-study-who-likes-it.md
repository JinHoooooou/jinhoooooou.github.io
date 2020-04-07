---
title: "Codewars 문제풀기 (03/08)"
excerpt: "Who likes it?"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-08
---



## [Who likes it?](https://www.codewars.com/kata/5266876b8f4bf2da9b000362/train/java)

* 문자열 배열을 인자로 받는다.
* 배열크기가 0 -> "no one likes this"
* 배열크기가 1 -> "이름 likes this"
* 배열크기가 2 -> "이름1 and 이름2 like this"
* 배열크기가 3 -> "이름1, 이름2 and 이름3 like this"
* 배열크기가 4이상 -> "이름1, 이름2 and 배열크기-2 others like this"


#### 1. Test를 만들었다

* 배열 크기가 0일때

  "no one likes this"출력

  ```java
  public static String whoLikesIt(Stirng... names) {
      return (names.length==0) ? "no one likes this" : "";
  }
  ```

* 배열 크기가 1일 때

  names[0] likes this 출력

  ```java
  public static String whoLikesIt(Stirng... names) {
      if(names.length == 0) {
          return "no one likes this";
      } else if(names.length == 1) {
          return names[0] + " likes this";
      }
      return "";
  }
  ```

* 배열 크기가 2 일 때

  names[0] and names[1] like this 출력

  ```java
  public static String whoLikesIt(Stirng... names) {
      if(names.length == 0) {
          return "no one likes this";
      } else if(names.length == 1) {
          return names[0] + " likes this";
      } else if(names.length == 2) {
          return names[0] + " and" + names[1] " like this";
      }
      return "";
  }
  ```

* 배열 크기가 3일 때

  names[0], names[1] and names[2] like this 출력

  ```java
  public static String whoLikesIt(Stirng... names) {
      if(names.length == 0) {
          return "no one likes this";
      } else if(names.length == 1) {
          return names[0] + " likes this";
      } else if(names.length == 2) {
          return names[0] + " and " + names[1] " like this";
      } else if(names.length == 3) {
          return names[0] + ", " + names[1] + " and " names[2] + " like this";
      }
      return "";
  }
  ```

* 배열 크기가 4이상 일 때

  names[0], names[1] and names.length-2 others like this 출력

  ```java
  public static String whoLikesIt(Stirng... names) {
      if(names.length == 0) {
          return "no one likes this";
      } else if(names.length == 1) {
          return names[0] + " likes this";
      } else if(names.length == 2) {
          return names[0] + " and " + names[1] " like this";
      } else if(names.length == 3) {
          return names[0] + ", " + names[1] + " and " names[2] + " like this";
      }
      return names[0] + ", " + names[1] + " and " + (names.length-2) + " others like this";
  }
  ```

  * Success

#### 2. 리팩토링 시작

switch랑 StringBuilder사용, 사실 연산이 많은편이 아니라 String을 써도 되지만 습관들일겸 사용

```java
public static String whoLikesIt(String... names) {
    StringBuilder result = new StringBuilder();
    switch (names.length) {
      case 0:
        result.append("no one likes this");
        break;
      case 1:
        result.append(names[0]).append(" likes this");
        break;
      case 2:
        result.append(names[0]).append(" and ").append(names[1]).append(" like this");
        break;
      case 3:
        result.append(names[0]).append(", ").append(names[1]).append(" and ").append(names[2])
            .append(" like this");
        break;
      default:
        result.append(names[0]).append(", ").append(names[1]).append(" and ")
            .append(names.length - 2).append(" others like this");
        break;
    }
    return result.toString();
  }
```



####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public static String whoLikesIt(String... names) {
        switch (names.length) {
          case 0: return "no one likes this";
          case 1: return String.format("%s likes this", names[0]);
          case 2: return String.format("%s and %s like this", names[0], names[1]);
          case 3: return String.format("%s, %s and %s like this", names[0], names[1], names[2]);
          default: return String.format("%s, %s and %d others like this", names[0], names[1], names.length - 2);
        }
}
```

* String.format을 사용하는게 더 보기 깔끔하다.


#### 4. 궁금한거 공부

* 문제를 보니 파라미터가 String...으로 되어있었다. 처음보는거라 검색해보니 가변인자라고한다.
* 가변인자(variable argument)
  * 자바5부터 매개변수의 개수를 가변인자를 이용해서 동적으로 지정해줄 수 있다.
  * 키워드 : ... (매개변수에서만 사용가능하고 변수선언때는 error가 나온다)
  * 가변인자는 내부적으로 배열을 생성하여 사용된다.
  * 가변인자 외 다른 매개변수가 있다면 가변인자는 마지막에 선언해야한다.

```java
void sum(String s, String... str) {
    
}
void sum(String... str) {
    
}
```

* 이런경우 컴파일러가 어떤 메소드를 사용해야하는지 구분을 못하여 error가 난다.
* 따라서 가변인자를 사용한 메소드 오버로딩은 가급적 사용하지 않는것이 좋다.