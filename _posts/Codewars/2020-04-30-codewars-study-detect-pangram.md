---
title: "Codewars 문제풀기 (04/30)"
excerpt: "Detect Pangram(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-30
---



# [Detect Pangram](https://www.codewars.com/kata/545cedaa9943f7fe7b000048/train/java)

* String을 인자로받는다.
* 입력 String이 Pangram인지 판별한다.
* Pangram은 a-z까지 모두 1개 이상 쓴 문장을 뜻한다. (Case insensitive)

```
check("The quick brown fox jumps over the lazy dog.") 👉 true
check("You shall not pass!") 👉 false
```

## 1. Test와 리팩토링

* ### 테스트 1 - 입력 String이 ""이라면 false를 리턴

  * 테스트 코드

    ```java
    @Test
    public void testShouldFalseWhenNotContainAtoZ() {
      // Given: Set empty string
      String given = "";
    
      // Then: Should False
      assertFalse(PangramChecker.check(given));
    }
    ```
    
    * When없이 Then으로 처리해도 괜찮다고 생각해서 없앴다.
    
  * 실제 코드
    
      ```java
      public class PangramChecker {
      
        public static boolean check(String sentence) {
          return false;
        }
      }
      ```

---


* ### 테스트 2 - 입력 String이 a-z 모두 있으면 true를 리턴

  * 테스트 코드

    ```java
    @Test
    public void testShouldTrueWhenNotContainAtoZ() {
      // Given: Set string contain a ~ z
      String given = "The quick brown fox jumps over the lazy dog.";
    
      // Then: Should True
      assertTrue(PangramChecker.check(given));
    }
    ```
    
  * 실제 코드
  
  ```java
    public class PangramChecker {
    
      public static boolean check(String sentence) {
        sentence = sentence.toLowerCase();
        for (char index = 'a'; index <= 'z'; index++) {
          if (!sentence.contains(String.valueOf(index))) {
            return false;
          }
        }
        return true;
      }
    }
    ```

* 이대로 제출했다(약 10분)

---

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class PangramChecker {
  public boolean check(String sentence){
        for (char c = 'a'; c<='z'; c++)
            if (!sentence.toLowerCase().contains("" + c))
                return false;
        return true;

  }
}
```

* `String.matches()`를 통해 정규식으로 해결할 수 있을거라 생각했는데, 의외로 내가 한 방법이 best practice였다.
