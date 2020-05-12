---
title: "Codewars 문제풀기 (05/12)"
excerpt: "Alternating Split (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-12
---



# [Alternating Split](https://www.codewars.com/kata/57814d79a56c88e3e0000786/train/java)

* String에서 짝수번째 char들을 먼저 쓰고 뒤에 나머지 char들을 붙여 만든 String을 리턴하는encrypt 메서드와 encrypt된 String을 복호하는 decrypt 메서드를 만들어라

* input String이 null이거나 empty("")라면 그대로 리턴한다.

* times가 0보다 작으면 input String을 그대로 리턴한다.

  ``` 
  encrypt("This is a Test!", 1) 👉 hsi  etTi sats!
  encrypt("This is a Test!", 2) 👉 s eT ashi tist!
  decrypt("s eT ashi tist!", 2) 👉 This is a Test!
  decrypt("This is a Test!, -1) 👉 This is a Test!
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - This is a test!, 1이 input일 때 "hsi  etTi sats!"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/27215325adffae604d73119aab0688ec8a1c25ce)

    ```java
    @Test
    @DisplayName("test should return hsi  etTi sats! when input string This is a test!, 1")
    public void test1() {
      // Given: Set string and n times
      String givenString = "This is a test!";
      int givenTimes = 1;
      // When: Call encrypt method
      String actual = Kata.encrypt(givenString, givenTimes);
      // Then: should return "hsi  etTi sats!"
      assertEquals("hsi  etTi sats!", actual);
    }
    ```
    
    
  
  
  - [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/ea138125d82b12ffc27f5308b08afd25490ee868)
  
    ```java
      public static String encrypt(final String text, final int n) {
        String tempText = text;
        String result = "";
        for (int i = 0; i < n; i++) {
          for (int j = 1; j < tempText.length(); j += 2) {
            result += "" + tempText.charAt(j);
          }
          for (int j = 0; j < tempText.length(); j += 2) {
            result += "" + tempText.charAt(j);
          }
          tempText = result;
        }
        return result;
      }
    ```
    
    * 이중 for문으로 해결했다.

* ### 테스트 2 - This is a test!, 3이 입력일 때 " Tah itse sits!"를 리턴한다

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/3d4f9d60747961bbbecbc4078a326b8529df04e4)

    ```java
    @Test
    @DisplayName("test should return  Tah itse sits! when input string This is a test!, 3")
    public void test2() {
      // Given: Set string and n times
      String givenString = "This is a test!";
      int givenTimes = 3;
      // When: Call encrypt method
      String actual = Kata.encrypt(givenString, givenTimes);
      // Then: should return " Tah itse sits!"
      assertEquals(" Tah itse sits!", actual);
    }
    ```
  
    
  
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/9e75ebadc061d4a3c5b0c84df77a03c1dc6122c1)
  
    ```java
      public static String encrypt(final String text, final int n) {
        String tempText = text;
        String result = "";
        for (int i = 0; i < n; i++) {
          result ="";
          for (int j = 1; j < tempText.length(); j += 2) {
            result += "" + tempText.charAt(j);
          }
          ...
    ```
    
    * result를 초기화 하지 않으면 계속 붙여 나가기 때문에 처음 for문에서 초기화 해준다.

* ### 테스트 3 - input times가 0 이하라면 input string을 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/994ac101e2186351054194c4701d094286e87c63)

    ```java
    @Test
    @DisplayName("test should return input string when times less than 0")
    public void test3() {
      // Given: Set string and n times that times less than 0
      String givenString = "This is a test!";
      int givenTimes = -1;
      // When: Call encrypt method
      String actual = Kata.encrypt(givenString, givenTimes);
      // Then: should return "This is a test"
      assertEquals("This is a test!", actual);
    }
    ```

    

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/f7f620dbc6c059affa3a63e01fa30b11b2bd23fc)

    ```java
      public static String encrypt(final String text, final int n) {
        if (text == null || n < 1) {
          return text;
        }
        String tempText = text;
        String result = "";
        for (int i = 0; i < n; i++) {
          ...
    ```

    * 예외처리

* ### 테스트 4 - hsi  etTi sats!, 1가 input이라면 This is a test!를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/9852590d989600e6d177c8828180e00709907e8a)

    ```java
    @Test
    @DisplayName("test should return This is a test! when input hsi  etTi sats!, 1")
    public void test4() {
      // Given: Set string and n times
      String givenString = "hsi  etTi sats!";
      int givenTimes = 1;
      // When: Call encrypt method
      String actual = Kata.decrypt(givenString, givenTimes);
      // Then: should return "This is a test"
      assertEquals("This is a test!", actual);
    }
    ```

    

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/4dafa3631561d25d2ac0d10ef94ee46d4f8d66b3)

    ```java
      public static String decrypt(final String encryptedText, final int n) {
        if (encryptedText == null || n < 1) {
          return encryptedText;
        }
        String tempText = encryptedText;
        String result = "";
        for (int i = 0; i < n; i++) {
          result = "";
          for (int j = 0, k = tempText.length() / 2; j < tempText.length() / 2; j++, k++) {
            result += "" + tempText.charAt(k) + tempText.charAt(j);
          }
          if (tempText.length() % 2 == 1) {
            result += "" + tempText.charAt(tempText.length() - 1);
          }
          tempText = result;
        }
        return result;
      }
    ```

    * 역시 2중 for문을 사용했다. encryptedText를 반으로 나눠 뒤쪽 char과 앞쪽 char를 순서대로 번갈아가며 result에 붙여줬다. string length가 홀수라면 마지막 char가 붙지 않으므로 마지막에 체크 후 붙여줬다.

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Kata {

  public static String encrypt(final String text, int n) {
                if (n <= 0 || text == null || text.isEmpty()) {
                        return text;
                }

                StringBuilder firstPart = new StringBuilder();
                StringBuilder secondPart = new StringBuilder();
                for (int i = 0; i < text.length(); i++) {
                        char aChar = text.charAt(i);
                        if (i % 2 == 1) {
                                firstPart.append(aChar);
                        } else {
                                secondPart.append(aChar);
                        }
                }

                return encrypt(firstPart.append(secondPart).toString(), --n);
        }

        public static String decrypt(final String encryptedText, int n) {
                if (n <= 0 || encryptedText == null || encryptedText.isEmpty()) {
                        return encryptedText;
                }

                StringBuilder text = new StringBuilder();
                final int half = encryptedText.length() / 2;
                for (int i = 0; i < half; i++) {
                        text.append(encryptedText.charAt(half + i)).append(encryptedText.charAt(i));
                }
                if (encryptedText.length() % 2 == 1) {
                        text.append(encryptedText.charAt(encryptedText.length() - 1));
                }

                return decrypt(text.toString(), --n);
        }
  
}
```

* 재귀를 이용해서 해결했다. 요즘 알고리즘 강의로 재귀를 좀 배우고 있는데 이 방법으로도 다시 한번 풀어봐야겠다. 재귀나 이중 for문이나 속도는 같은데 이 코드가 조금 더 깔끔하긴 하다.

