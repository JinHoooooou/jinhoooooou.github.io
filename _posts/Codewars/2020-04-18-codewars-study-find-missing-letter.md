---
title: "Codewars 문제풀기 (04/18)"
excerpt: "Find Missing Letter(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-18
---



## [Find Missing Letter](https://www.codewars.com/kata/5839edaa6754d6fec10000a2/train/java)

* char 배열을 인자로 받는다.
* char 배열은 연속된 문자열이며, 반드시 한 문자가 빠져있다. 그리고 대문자로 시작하면 대문자, 소문자로 시작하면 소문자이다. 
* 배열의 길이는 항상 2 이상이다
* ["a","b","c","d","f"] 👉 "e"
* ["O","Q","R","S"] 👉 "P"

#### 1. Test와 리팩토링

* 테스트 1 - 입력 배열이 [a, c]라면 b를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 'b' when input is {a, c}")
    public void testShouldReturnBWhenInputIsAAndC() {
      // Given: Set array {a, c}
      char[] given = {'a', 'c'};
      // When: Call findMissingLetter method
      char actual = Kata.findMissingLetter(given);
      // Then: Should return 'b'
      assertEquals('b', actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class Kata {
    
      public static char findMissingLetter(char[] array) {
        return 'b';
      }
    }
    
    ```

  * 리팩토링 1 - 입력 배열 길이만큼 반복문을 돌며 비어있는 문자를 찾는다.

    ```java
    public class Kata {
    
      public static char findMissingLetter(char[] array) {
        result = array[0];
        for(int i = 0; i<array[i]; i++) {
          if(result != array[i]) {
            break;
          }
          result++;
        }
        return result;
      }
    }
    ```

  * 리팩토링 2 - 향상된 for문 사용

    ```java
    public class Kata {
    
      public static char findMissingLetter(char[] array) {
        result = array[0];
        for(char letter : array) {
          if(result != letter) {
            break;
          }
          result++;
        }
        return result;
      }
    }
    ```

    

* 테스트 2 - 입력 배열이 [O, P, R, S, T, U]라면 Q를 리턴한다.

  - 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 'Q' when input is {O, P, R, S, T}")
    public void testShouldReturnEWhenInputIsAAndBAndCAndDAndF() {
      // Given: Set array {O, P, R, S, T}
      char[] given = {'O', 'P', 'R', 'S', 'T'};
      // When: Call findMissingLetter method
      char actual = Kata.findMissingLetter(given);
      // Then: Should return 'Q'
      assertEquals('Q', actual);
    }
    ```

  * 실제 코드 - 그대로


* 제출 (약 10분)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Kata {
  public static char findMissingLetter(char[] array){
    char expectableLetter = array[0];
    for(char letter : array){
      if(letter != expectableLetter) break;
      expectableLetter++;
    }
    return expectableLetter;
  }
}
```

* 완전 같은 방법으로 구현했다.

신박하다고 생각한 코드

```java
public class Kata {
  public static char findMissingLetter(char[] array)
  {
    for (int i = 1; i < array.length ; i++){
      if(array[i] - array[i-1] != 1){
        return (char)(array[i-1]+1); 
      }
    }
    throw new IllegalArgumentException("Should not happen!");
  }
}
```

* `array[i] - array[i-1] != 1`가 두 문자가 연속되지 않았다는 뜻이니까 그 사이의 문자를 리턴하면 되는구나