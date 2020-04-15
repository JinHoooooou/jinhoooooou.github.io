---
title: "Codewars 문제풀기 (04/15)"
excerpt: "Simple Pig Latin(5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-15
---



## [Simple Pig Latin](https://www.codewars.com/kata/520b9d2ad5c005041100000f/train/java)

* String을 인자로 받는다.
* String의 각 단어의 첫 letter를 끝으로 옮기고 "ay"를 붙인 String을 반환한다. 
* !, ? 등 기호가 붙어있다면 그대로 둔다.
* pigIt("Pig latin is cool") --> "igPay atinlay siay oolcay"
* pigIt("Hello world !") --> "elloHay orldway !"

#### 1. Test와 리팩토링

* 테스트 1 - 입력 String이 기호가 아닌 1 letter인경우 letter에 "ay"만 붙여 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnJustAddStringAyWhenInputOneLetter() {
      // Given: Set string one letter not punctuation
      String given = "j";
      // When: Call pigIt method
      String actual = PigLatin.pigIt(given);
      // Then: Should return "jay"
      assertEquals("jay", actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class PigLatin {
    
      public static String pigIt(String str) {
        return str + "ay";
      }
    }
    ```

* 테스트 2 - 입력 String이 기호인 1 letter인 경우 그냥 letter를 리턴한다.

  - 테스트 코드

    ```java
    @Test
    public void testShouldLeavePunctuation() {
      // Given: Set string one punctuation
      String given = "!";
      // When: Call pigIt method
      String actual = PigLatin.pigIt(given);
      // Then: Should return "jay"
      assertEquals("!", actual);
    }
    ```

  * 실제 코드

    ```java
    public class PigLatin {
    
      public static String pigIt(String str) {
        if (str.matches(str.matches("[a-zA-z0-9]+")) {
          return str + "ay";
        }
        return str;
      }
    }
    ```
    
    * matches() 메서드를 이용해서 a~Z, 0~9문자가 포함되어있으면 "ay"를 붙이고 그게 아니라면 기호문자이므로 그대로 리턴하도록 했다.
    

* 테스트 3 - 입력 String이 1개의 단어라면 첫번째 letter를 단어 끝에 붙이고 "ay"를 붙여 리턴한다.

  * 테스트 코드

    ```java
     @Test
    public void testShouldMoveFirstLetterToEndOfWordAndAddAyWhenInputIsMoreThanTwoLetters() {
      // Given: Set string one word
      String given = "jinho";
      // When: Call pigIt method
      String actual = PigLatin.pigIt(given);
      // Then: Should return "inhojay"
      assertEquals("inhojay", actual);
    }
    ```

  * 실제 코드

    ```java
    public class PigLatin {
    
      public static String pigIt(String str) {
        if (str.matches("[a-zA-z0-9]+")) {
            return str.substring(1) + str.substring(0, 1) + "ay";
          }
        }
        return str;
      }
    }
    ```

    * str이 1 word일 때의 테스트 이므로, 기호문자가 없다면 첫 문자를 문자열 끝으로 보낸 후 "ay"를 붙여주었다.

* 테스트 4 - 입력 String이 2개 이상의 단어라면 모든 단어의 첫번째 letter를 단어 끝에 붙이고 "ay"를 붙여 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldMoveFirstLetterToEndOfEachWordAndAddAyWhenInputIsMoreThanTwoWords() {
      // Given: Set string three words not contain punctuation
      String given = "jinho is king";
      // When: Call pigIt method
      String actual = PigLatin.pigIt(given);
      // Then: Should return "inhojay siay ingkay"
      assertEquals("inhojay siay ingkay", actual);
    }
    ```

  * 실제 코드

    ```java
    public class PigLatin {
    
      public static String pigIt(String str) {
        String[] words = str.split(" ");
        for (int i = 0; i < words.length; i++) {
          if (words[i].matches("[a-zA-z0-9]+")) {
            words[i] = words[i].substring(1) + words[i].substring(0, 1) + "ay";
          }
        }
        return String.join(" ", words);
      }
    }
    ```

    * 문자열을 " "로 쪼개 각 문자열마다 첫 문자를 문자열 끝에 붙여주었다. 그리고 join메서드를 통해 String 배열을 합쳐주었다.

* 리팩토링

  1. for문 내에 mathces로 검증 후 문자열을 붙이는 로직을 메서드로 추출했다.

     ```java
     public class PigLatin {
     
       public static String pigIt(String str) {
         String[] words = str.split(" ");
         for (int i = 0; i < words.length; i++) {
           words[i] = makePigLatinEachWord(words[i]);
         }
         return String.join(" ", words);
       }
     
       private static String makePigLatinEachWord(String word) {
         if (word.matches("[a-zA-z0-9]+")) {
           return word.substring(1) + word.substring(0, 1)
               + "ay";
         }
         return word;
       }
     }
     ```

     * 처음에 `makePigLatinEachWord()`메서드의 이름을 문자열에 기호 문자가 없다면 PigLatin을 만들고 있다면 그냥 반환한다.. 라는 식으로 이름을 지을까 했는데, pigLatin자체가 그 기능을 얘기하고 있기 때문에 그냥 간단하게 이름지었다.

  2.  오버 같긴한데 substring(1)이랑 "ay"를 상수로 추출했다.

     ```java
     public class PigLatin {
     
       static final String ADD_LETTERS = "ay";
       static final int MOVE_LETTER_LENGTH = 1;
     
       public static String pigIt(String str) {
         ...
       }
     
       private static String makePigLatinEachWord(String word) {
         if (word.matches("[a-zA-z0-9]+")) {
           return word.substring(MOVE_LETTER_LENGTH) + word.substring(0, MOVE_LETTER_LENGTH)
               + ADD_LETTERS;
         }
         return word;
       }
     }
     ```

     * 이 문제에서는 첫번째 문자를 문자열 끝에 붙이고 "ay"를 붙여준다지만, "ay"가 다른 문자열로 바뀔수도 있는거고.. 문자열 끝에 붙이는 문자가 1개가 아니라 2개이상이 될 수도 있는거니까.. 상수로 추출해봤다. 

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class PigLatin {
    public static String pigIt(String str) {
        return str.replaceAll("(\\w)(\\w*)", "$2$1ay");
    }
}
```

* 엄청 간단하게 풀었다. `replaceAll()` 메서드 생각을 못했다. 근데 regax는 이해를 못하겠다. 
* \w는 단어를 만들 수 있는 글자. 알파벳 대소문자. 숫자, 언더스코어가 포함된다.
* 근데 (\w)(\w*)은 잘은 모르겟는데 단어를 만들수 있는 글자가 0개 이상인걸 나타내는것 같다.
* "$2$1ay"는 진짜 뭔지 모르겠다.. 댓글에도 없고 검색해도 잘 모르겠다 ㅜ



* 답과 별개로 테스트 코드에 대한 실제 코드를 구현 하고 테스트가 통과하면 바로 다음 테스트를 하는 경향이 있다. 그러고 나서 모든 테스트가 통과하면 그 때 리팩토링을 하려한다. 테스트 코드를 통과하면 리팩토링을 바로하는 습관을 가지자

