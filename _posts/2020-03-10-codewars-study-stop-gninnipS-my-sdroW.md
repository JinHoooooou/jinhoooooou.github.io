---
title: "Codewars 문제풀기 (03/10)"
excerpt: "Stop gninnipS My sdroW!"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-10
---



## [Stop gninnipS My sdroW!](https://www.codewars.com/kata/5264d2b162488dc400000001/train/java)

* 문자열을 인자로 받는다.
* space(" ")를 기준으로 각 단어의 길이가 5이상이면 단어를 뒤집는다.
* 5보다 작다면 그대로 출력한다.
* spinWords("This is a test") --> This is a test
* spinWords("This is another test") --> this is rehtona test


#### 1. Test를 만들었다

* 문장의 각 단어의 길이가 모두 4이하 일 때

  인자로 입력한 문자열 그대로 출력

  ```java
  public class SpinWords {
    public static String spinWords(String sentence) {
        return sentence;
    }
  ```

* 배열 크기가 1일 때

  `String.split()`으로 단어별로 쪼갠다음 단어 길이가 5 이상이면 단어 배열을 바꾸는 방법으로 해결

  ```java
  public class SpinWords {
    public static String spinWords(String sentence) {
      String[] words = sentence.split(" ");
      StringBuilder result = new StringBuilder();
      for (String word : words) {
       if (word.length() >= 5) {
        char[] reverseWord = new char[word.length()];
        for (int i = 0; i < word.length(); i++) {
          reverseWord[i] = word.charAt(word.length() - (i + 1));
        }
        word = String.valueOf(reverseWord);
       }
        result.append(word).append(" ");
      }
      return result.deleteCharAt(result.length() - 1).toString();
    }
  ```

  Success


#### 2. 리팩토링 시작

단어를 reverse하는 과정을 메소드로 추출

```java
public class SpinWords {
  public static String spinWords(String sentence) {
    String[] words = sentence.split(" ");
    StringBuilder result = new StringBuilder();
    for (String word : words) {
      word = checkAndReverseWord(word);
      result.append(word).append(" ");
    }
    return result.deleteCharAt(result.length() - 1).toString();
  }

  private static String checkAndReverseWord(String word) {
    if (word.length() >= 5) {
      char[] reverseWord = new char[word.length()];
      for (int i = 0; i < word.length(); i++) {
        reverseWord[i] = word.charAt(word.length() - (i + 1));
      }
      word = String.valueOf(reverseWord);
    }
    return word;
  }
```

* 보기는 좋다.
* 근데 문자열을 reverse하는 함수를 따로 지원하지 않을까? 하고 찾아보니 StringBuilder와 StringBuffer 클래스에 reverse() 메소드가 존재한다. 이것을 이용하면 더 간단할것 같다.

```java
public class SpinWords {
  public static String spinWords(String sentence) {
    String[] words = sentence.split(" ");
    StringBuilder result = new StringBuilder();
    for (String word : words) {
      word = new StringBuilder(word).reverse().toString();
      result.append(word).append(" ");
    }
    return result.deleteCharAt(result.length() - 1).toString();
  }
```

* 훨씬 간결해졌다. 

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class SpinWords {

  public String spinWords(String sentence) {
    String[] words = sentence.split(" ");
    for (int i=0; i<words.length; i++) {
      if (words[i].length() >= 5) {
        words[i] = new StringBuilder(words[i]).reverse().toString();
      }
    }
    return String.join(" ",words);
  }
}
```

* StringBuilder 클래스의 append를 이용해서 새로 문자열을 만든것이 아니라 split()으로 쪼갠 문자열을 join 메소드로 붙였다. 이 방법이 더 깔끔해서 보기 좋은것 같다.

Stream API 이용한 코드

```java
import java.util.stream.*;
import java.util.Arrays;

public class SpinWords {

  public String spinWords(String sentence) {
    return Arrays.stream(sentence.split(" "))
                 .map(i -> i.length() > 4 ? new StringBuilder(i).reverse().toString() : i)
                 .collect(Collectors.joining(" "));
  }
}
```

* 스트림이 만능은 아니지만 이런 간단한 문제들은 함수형 프로그래밍으로 해결하려고 노력을 해야할것같다. 스트림 공부가 아직 덜 되었기때문에 익숙하지않다 ㅜ


#### 4. 궁금한거 공부

* 없음 