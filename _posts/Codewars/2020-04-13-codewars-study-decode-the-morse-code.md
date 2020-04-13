---
title: "Codewars 문제풀기 (04/13)"
excerpt: "Decode the Morse code(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-13
---



## [Decode the Morse code](https://www.codewars.com/kata/54b724efac3d5402db00065e/train/java)

* 모스 부호로 이루어진 String을 인자로 받는다.
* 모스 부호를 영문자로 바꾸는 메서드를 구현한다.
* decode(".... . -.--   .--- ..- -.. .") --> "HEY JUDE"
* 출력문자는 대문자로 가정한다.
* 모스 부호는 문자간 space 1개 단어간 space 3개로 구분한다.

#### 1. Test와 리팩토링

* 테스트 1 - 입력 인자가 1개의 문자를 나타내는 모스 부호일 경우 문자 하나만 리턴한다. (A, B, C, 1, 2 등..)

  * 테스트 코드

    ```java
    @Test
    public void testShouldDecodeAlphabet() {
      // Given: Set String one character
      String given = ".-";
      // When: Call decode method
      String actual = MorseCodeDecoder.decode(given);
      // Then: Should Return A
      assertEquals("A", actual);
    }
    ```
    
  * 실제 코드

    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class MorseCodeDecoder {
    
      static Map<String, String> morseCodeMap = new HashMap<>();
    
      public static String decode(String morseCode) {
        makeMorseCodeMap();
        return morseCodeMap.get(morseCode);
      }
    
      private static void makeMorseCodeMap() {
        morseCodeMap.put(".-", "A");
        morseCodeMap.put("-...", "B");
        morseCodeMap.put("-.-.", "C");
        morseCodeMap.put("-..", "D");
        morseCodeMap.put(".", "E");
        morseCodeMap.put("..-.", "F");
        morseCodeMap.put("--.", "G");
        morseCodeMap.put("....", "H");
        morseCodeMap.put("..", "I");
        morseCodeMap.put(".---", "J");
        morseCodeMap.put("-.-", "K");
        morseCodeMap.put(".-..", "L");
        morseCodeMap.put("--", "M");
        morseCodeMap.put("-.", "N");
        morseCodeMap.put("---", "O");
        morseCodeMap.put(".--.", "P");
        morseCodeMap.put("--.-", "Q");
        morseCodeMap.put(".-.", "R");
        morseCodeMap.put("...", "S");
        morseCodeMap.put("-", "T");
        morseCodeMap.put("..-", "U");
        morseCodeMap.put("...-", "V");
        morseCodeMap.put(".--", "W");
        morseCodeMap.put("-..-", "X");
        morseCodeMap.put("-.--", "Y");
        morseCodeMap.put("--..", "Z");
        morseCodeMap.put(".----", "1");
        morseCodeMap.put("..---", "2");
        morseCodeMap.put("...--", "3");
        morseCodeMap.put("....-", "4");
        morseCodeMap.put(".....", "5");
        morseCodeMap.put("-....", "6");
        morseCodeMap.put("--...", "7");
        morseCodeMap.put("---..", "8");
        morseCodeMap.put("----.", "9");
        morseCodeMap.put("-----", "0");
      }
    }
    
    ```
    
    * 위키피디아를 보고 모스부호 맵을 만들었다. 알파벳에 대한 테스트 때문에 A~Z, 0~9까지 추가했다.

* 테스트 2 - 입력인자가 1개의 단어를 나타내는 모스부호일 경우 복호한 단어를 리턴해야한다.

  - 테스트 코드

    ```java
    @Test
    public void testShouldDecodeMoreThan2Words() {
      // Given: Set String one word
      String given = ".--- .. -. .... --- ....-   -... .- -... ---";
      // When: Call decode method
      String actual = MorseCodeDecoder.decode(given);
      // Then: Should Return JINHO4 BABO
      assertEquals("JINHO4 BABO", actual);
    }
    ```

  * 실제 코드

    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class MorseCodeDecoder {
    
      static Map<String, String> morseCodeMap = new HashMap<>();
    
      public static String decode(String morseCode) {
        makeMorseCodeMap();
        String[] eachCharacter = morseCode.split(" ");
        String result = "";
        for(String character : eachCharacter) {
          result += morseCodeMap.get(character);
        }
        return result;
      }
    }
    ```

    * 모스부호 문자간 1개의 space로 구분되어있기 때문에 split(" ")으로 각 문자를 추출 후 result에 붙였다.

* 테스트 3 - 입력인자가 2개이상의 단어를 나타내는 모스부호일 경우, 단어들을 복호하여 리턴한다. 

  * 테스트 코드

    ```java
    @Test
    public void testShouldDecodeOneWord() {
      // Given: Set String one word
      String given = ".--- .. -. .... --- ....-";
      // When: Call decode method
      String actual = MorseCodeDecoder.decode(given);
      // Then: Should Return JINHO4
      assertEquals("JINHO4", actual);
    }
    ```

  * 실제 코드

    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class MorseCodeDecoder {
    
      static Map<String, String> morseCodeMap = new HashMap<>();
    
      public static String decode(String morseCode) {
        makeMorseCodeMap();
        String[] morseWords = morseCode.split("   ");
        String result = "";
        for (String morseWord : morseWords) {
          String[] morseChars = morseWord.split(" ");
          for (String morseChar : morseChars) {
            result += morseCodeMap.get(morseChar);
          }
          result += " ";
        }
        return result.trim();
      }
    }
    
    ```

    * 모스부호 단어간 space 3개로 구분지어져있기 때문에 split("   ")으로 각 모스부호 단어를 추출 후 단어마다 다시 문자를 추출하여 result에 붙였다.

  * 리팩토링 - 각 문자 복호는 함수로 추출, String대신 StringBuilder 사용

    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class MorseCodeDecoder {
    
      static Map<String, String> morseCodeMap = new HashMap<>();
    
      public static String decode(String morseCode) {
        makeMorseCodeMap();
        String[] morseWords = morseCode.trim().split("   ");
        StringBuilder result = new StringBuilder();
        for (String morseWord : morseWords) {
          result.append(decodeMorseWord(morseWord)).append(" ");
        }
        return result.toString().trim();
      }
      private static String decodeMorseWord(String morseWord) {
        StringBuilder result = new StringBuilder();
        String[] morseCharacters = morseWord.split(" ");
        for (String morseCharacter : morseCharacters) {
          result.append(morseCodeMap.get(morseCharacter));
        }
        return result.toString();
      }
    }
    ```
    * 제출(리팩토링 제외 약 30분 소요)


#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class MorseCodeDecoder {
    public static String decode(String morseCode) {
      String result = "";
      for(String word : morseCode.trim().split("   ")) {
        for(String letter : word.split("\\s+")) {
          result += MorseCode.get(letter);
        }
        result += ' ';
      }
      return result.trim();
    }
}
```

* 문제 자체는 같은 방식으로 해결했는데 split()메서드에서 space를 기준으로 문자들을 쪼갠것이 아니고 "\\s+"로 공백이 없는 문자열 기준으로 쪼갰다.



