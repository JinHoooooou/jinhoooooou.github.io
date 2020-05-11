---
title: "Codewars 문제풀기 (05/11)"
excerpt: "Valid Braces (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-11
---



# [Valid Braces](https://www.codewars.com/kata/5277c8a221e209d3f6000b56/train/java)

* String을 인자로 받는다.

* String은 괄호 (), {}, []등으로 이루어져 있다.

* 괄호 형태가 이상하면 false를 맞다면 true를 리턴한다.

  ``` 
  (){}[] 👉 true
  ([{}]) 👉 true
  {) 👉 false
  [({})](] 👉 false
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 {), [}, (] 등 형식이 맞지 않으면 false를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("(], {]등 괄호가 쌍이 아니라면 false")
    public void test1() {
      // Given: Set braces
      String given = "{]";
      // Then: should false
      assertFalse(braceChecker.isValid(given));
    }
    ```
  
  
  - 실제 코드
  
    ```java
    public class BraceChecker {
    
      public boolean isValid(String braces) {
        
        for (int i = 0, j = braces.length() - 1; i < braces.length() / 2; i++, j--) {
          switch (braces.charAt(i)) {
            case '(':
              if (braces.charAt(j) != ')') {
                return false;
              }
              break;
            case '{':
              if (braces.charAt(j) != '}') {
                return false;
              }
              break;
            case '[':
              if (braces.charAt((j)) != ']') {
                return false;
              }
              break;
            default:
              return false;
          }
        }
        return true;
      }
    }
    
    ```
  
    * String을 반으로 나누어 switch/case문으로 String 앞부분에서 닫는 괄호( }, ), ] 등) 이면 잘못된 괄호이기 때문에 false를 리턴하도록 구현했다.
    * 이렇게 구현하면 {},(), {([])} 등 겹치는 괄호도 패스가 가능하다.
  
* ### 테스트 2 - 입력이 (})등 짝이 없다면 false를 리턴한다.

  * 테스트 코드

    ```java
  @Test
    @DisplayName("(})처럼 괄호 짝이 없다면 false")
  public void test4() {
      // Given: Set braces
      String given = "(})";
      // Then: should false
      assertFalse(braceChecker.isValid(given));
    }
    ```
  
    
  
  * 실제 코드
  
    ```java
    public class BraceChecker {
    
      public boolean isValid(String braces) {
    
        if (braces.length() % 2 == 1) {
          return false;
        }
    
        for (int i = 0, j = braces.length() - 1; i < braces.length() / 2; i++, j--) {
          switch (braces.charAt(i)) {
            case '(':
              if (braces.charAt(j) != ')') {
                return false;
              }
              break;
            case '{':
              if (braces.charAt(j) != '}') {
                return false;
              }
              break;
            case '[':
              if (braces.charAt((j)) != ']') {
                return false;
              }
              break;
            default:
              return false;
          }
        }
        return true;
      }
    }
    ```
  
    * String길이가 홀수이면 짝이 안맞게 되니 false를 리턴하도록 구현했다.

### 실패.. - {}()[]{}처럼 완성된 괄호가 여러개 나열되어 있는 경우를 통과하지 못함

처음에 switch / case로 구현해보려고 했는데 계속 실패하고 시간만 잡아먹고 그래서 포기했다 ㅜ



---

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Stack;

public class BraceCheckerBestPractice {

  public boolean isValid(String braces) {
    Stack<Character> s = new Stack<>();
    for (char c : braces.toCharArray()) {
      if (s.size() > 0 && isClosing(s.peek(), c)) {
        s.pop();
      } else {
        s.push(c);
      }
    }
    return s.size() == 0;
  }

  public boolean isClosing(char x, char c) {
    return (x == '{' && c == '}') || (x == '(' && c == ')') || (x == '[' && c == ']');
  }
}

```

* Stack을 사용해서 구현했는데, 이 방법을 생각하지 못했다 ㅜㅜㅜㅜ. 전에 West, North, South, East 방향 구하는 문제 풀 때도 Stack사용하는 문제가 있었는데 그걸 보고도 못한게 너무 아쉽다 ㅜㅜㅜㅜㅜ

