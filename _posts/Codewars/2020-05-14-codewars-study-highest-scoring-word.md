---
title: "Codewars 문제풀기 (05/14)"
excerpt: "Highest Scoring Word (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-14
---



# [Highest Scoring Word](https://www.codewars.com/kata/57eb8fcdf670e99d9b000272/train/java)

* String을 입력으로 받는다.

* String의 words중 a=1, b=2, c=3...점으로하여 점수가 가장 높은 단어를 리턴한다.

* 모든 단어는 소문자로 이루어져있다.

  ``` 
  Kata.high("man i need a taxi up to ubud") 👉 taxi
  Kata.high("what time are we climbing up to the volcano") 👉 volcano
  ```
  



## 1. Test와 리팩토링

* ### 테스트 1 - "man i need a taxi up to ubud"가 입력일때 "taxi"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/816b248d6f1c3b2bda00fd5b3dc4a604afa88763)

    ```java
    @Test
    @DisplayName("test should return taxi when input is man i need a taxi up to ubud")
    public void test1() {
      // Given: Set string
      String given = "man i need a taxi up to ubud";
      // When: call high method
      String actual = Kata.high(given);
      // Then: Should return taxi
      assertEquals("taxi", actual);
    }
    ```
    
    
    
    * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/dc1659b1658684bf2759d8b09846dc4218171f27)

      ```java
      public class Kata {
      
        public static String high(String s) {
          String[] split = s.split(" ");
          int max = 0;
          String result = "";
          for (String word : split) {
            int wordScore = 0;
            for (int i = 0; i < word.length(); i++) {
              wordScore += word.charAt(i) - 96;
            }
            if (wordScore > max) {
              max = wordScore;
              result = word;
            }
          }
          return result;
        }
      }
      ```

      * String을 space(" ")로 쪼개어 각 단어의 문자 score를 더해준 뒤 비교해서 점수가 가장 높은 word를 리턴했다. 96을 빼준 이유는 char a값이 97이기 때문이다.

        

  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/2d7bc7ea8bc69bc05aba068b406504dfbb363866)

    ```java
    public class Kata {
    
      public static String high(String s) {
    		int highest = 0;
        String result = "";
        for (String word : s.split(" ")) {
          int wordScore = getWordScore(word);
          if (wordScore > highest) {
            highest = wordScore;
            result = word;
          }
        }
        return result;
      }
      
      private static int getWordScore(String word) {
        int wordScore = 0;
        for (int i = 0; i < word.length(); i++) {
          wordScore += word.charAt(i) - 96;
        }
        return wordScore;
      }
    }
    ```

    * word의 score를 구하는 부분을 함수로 추출했다.
    * max라는 이름보다 higest가 더 어울리는거 같아 수정했다.

* ### 테스트 2 - "what time are we climbing up to the volcano"가 입력일 때 "volcano"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/950d3070180453c06fc04737bc61c08bb35103fb)

    ```java
    @Test
    @DisplayName("test should return volcano when input is what time are we climbing up to the volcano")
    public void test2() {
      // Given: Set string
      String given = "what time are we climbing up to the volcano";
      // When: call high method
      String actual = Kata.high(given);
      // Then: Should return volcano
      assertEquals("volcano", actual);
    }
    ```
    

  * 실제 코드 그대로



## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.*;

public class Kata {
  public static String high(String s) {
    return Arrays.stream(s.split(" "))
                .max(Comparator.comparingInt(
                        a -> a.chars().map(i -> i - 96).sum()
                )).get(); 
  }
}
```

* Stream으로 해결했다. 처음에 생각은 했는데, score를 구하고 비교하는 부분을 어떻게 해줘야할지 몰라서 그냥 이중 for문으로 해결했었다. `Comparator.comparingInt()`를 몰랐다...



