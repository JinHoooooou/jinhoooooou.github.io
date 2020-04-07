---
title: "Codewars 문제풀기 (03/23)"
excerpt: "Duplicate Encoder"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-23
---



## [Duplicate Encoder](https://www.codewars.com/kata/54b42f9314d9229fd6000d9c/train/java)

* String을 인자로 받는다.
* 겹치는 문자가 있으면 ')'로 변환, 겹치지 않는 문자가 있으면 '('로 변환하여 리턴한다.
* 대소문자는 구분하지않는다.

* "din" -->  "((("
* "recede"   -->  "()()()"
*  "Success"  -->  ")())())" 
* "(( @"     -->  "))((" 

#### 1. Test를 만들었다.

* 모든 문자가 겹치지 않을 때 '('로 변환해야한다.

  * 테스트 코드
  
    ```java
      public void testShouldReturnOpenWhenAllCharactersAreNotDuplicate() {
        //Given : Set string all characters not duplicate
        String given = "abcdefghi";
        //When : Call encode
        String actual = DuplicateEncoder.encode(given);
        //Then : Should return "((((((((("
        assertEquals("(((((((((", actual);
      }
    ```
    
  * 실제 코드
  
    ```java
    public class DuplicateEncoder {
    
      public static String encode(String word) {
        char[] toArray = word.toCharArray();
        for (int i = 0; i < toArray.length; i++) {
          char target = toArray[i];
          for (int j = i + 1; j < toArray.length; j++) {
            if (target == toArray[j]) {
              toArray[j] = ')';
              toArray[i] = ')';
            }
          }
          if (toArray[i] != ')') {
            toArray[i] = '(';
          }
        }
        return new String(toArray);
      }
    }
    ```

* 모든 문자가 겹칠 때 ')'로 변환해야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnCloseWhenAllCharactersAreDuplicate() {
        //Given : Set string all characters not duplicate
        String given = "abccbabca";
        //When : Call encode
        String actual = DuplicateEncoder.encode(given);
        //Then : Should return "((((((((("
        assertEquals(")))))))))", actual);
      }
    ```

  * 실제코드 그대로

* 대소문자는 구별하지 않는다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldBeCaseInsensitive() {
        //Given : Set string all characters not duplicate
        String given = "Prespecialized";
        //When
        String actual = DuplicateEncoder.encode(given);
        //Then
        assertEquals(")()())()(()()(", actual);
      }
    ```

  * 실제 코드

    ```java
    public class DuplicateEncoder {
    
      public static String encode(String word) {
        word = word.toLowerCase();
        char[] toArray = word.toCharArray();
        for (int i = 0; i < toArray.length; i++) {
          char target = toArray[i];
          for (int j = i + 1; j < toArray.length; j++) {
            if (target == toArray[j]) {
              toArray[j] = ')';
              toArray[i] = ')';
            }
          }
          if (toArray[i] != ')') {
            toArray[i] = '(';
          }
        }
        return new String(toArray);
      }
    }
    ```

    * `word = word.toLowerCase()`추가
    * 여기까지 생각하고 제출했는데 Fail이 떴다. "----()(---"테스트를 통과하지 못했다.

* ')', '('문자가 들어간 경우

  * 테스트 코드

    ```java
      @Test
      public void testWhenContainOpenAndCloseMark() {
        //Given : Set string all characters not duplicate
        String given = "---()(---";
        //When
        String actual = DuplicateEncoder.encode(given);
        //Then
        assertEquals("))))())))", actual);
      }
    ```

    

  * 실제 코드

    ```java
    public class DuplicateEncoder {
      static String encode(String word){
        word = word.toLowerCase();
    
        char[] encodingChar = new char[word.length()];
        char[] toArray = word.toCharArray();
        for (int i = 0; i < toArray.length; i++) {
          if (encodingChar[i] != '\0') {
            continue;
          }
          char target = toArray[i];
          for (int j = i + 1; j < toArray.length; j++) {
            if (target == toArray[j]) {
              encodingChar[j] = ')';
              encodingChar[i] = ')';
            }
          }
          if (encodingChar[i] != ')') {
            encodingChar[i] = '(';
          }
        }
        return new String(encodingChar);
      }
    }
    ```

    * 여기서 처음에 문자열을 '변환'한다는 생각에 덮어쓰려고 했었다.
    * 그래서 계속 fail이 떴는데 리턴할 배열을 따로 선언해서 구현하니 통과했다.
    * 이 과정에서 시간을 많이 썼다 ㅜㅜ

* 리팩토링 고민하다가 문제 푸는것도 오래걸렸고, 생각도 안나서 그냥 제출했다.

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class DuplicateEncoder {
  static String encode(String word){
    word = word.toLowerCase();
    String result = "";
    for (int i = 0; i < word.length(); ++i) {
      char c = word.charAt(i);
      result += word.lastIndexOf(c) == word.indexOf(c) ? "(" : ")";
    }
    return result;
  }
}
```

* `lastIndexOf()`메서드는 문자열에서 해당 문자의 가장 마지막 index를 반환한다.  "abcabcabc"에서 lastIndexOf('a')는 6이고, indexOf('a')는 0이다.
* 문자열에서 문자가 하나라면 lastIndexOf() == indexOf()이다. 그래서 반환할 문자열에 "("를 붙여주면된다. 같지 않다면 ")"를 붙여준다.
* 일단 문제를 푸는데도 오래걸려서 더 간단하게 푸는 방법을 아에 생각하지못했다. 그리고 문제를 "변환"한다는 생각에 `replace`메소드만 생각하거나, 인자 문자열에 덮어쓰는것만 생각했다. 문제를 좀더 유연하게 바라볼 필요가 있는것 같다 ㅜㅜ
