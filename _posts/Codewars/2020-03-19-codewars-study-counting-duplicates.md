---
title: "Codewars 문제풀기 (03/19)"
excerpt: "Counting Duplicates"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-19
---



## [Counting Duplicates](https://www.codewars.com/kata/54bf1c2cd5b56cc47f0007a1/train/java)

* String을 인자로 받는다.
* 인자에서 대소문자 상관없이 같은 문자가 있는 문자의 개수를 반환한다.
* duplicateCount(abcde) --> 0
* duplicateCount(aabbcde) --> a,b가 겹치므로 2
* duplicateCount(aabBcde) --> 대소문자 관계 없으므로 2
* duplicateCount(indivisibility) --> i만 6번 겹치므로 1


#### 1. Test를 만들었다

* 입력 인자가 모두 구별된 문자일 경우

  * 테스트 코드

  ``` java
    @Test
    public void testWhenNotDuplicateShouldReturnZero() {
      //Given
      String given = "abcde";
      //When
      int actual = CountingDuplicates.duplicateCount(given);
      //Then
      assertEquals(0, actual);
    }
  ```
  
  * 실제 코드
  
  ```java
  public class CountingDuplicates {
  
    public static int duplicateCount(String text) {
      return 0;
    }
  }
  ```
  
* 입력 인자가 1개 겹칠 경우

  * 테스트 코드

  ```java
    @Test
    public void testWhenOnlyOneCharacterDuplicateShouldReturnOne() {
      //Given
      String given = "indivisibility";
      //When
      int actual = CountingDuplicates.duplicateCount(given);
      //Then
      assertEquals(1, actual);
    }
  
  ```
  
  * 실제 코드
  
  ```java
  public class CountingDuplicates {
  
    public static int duplicateCount(String text) {
      String[] character = text.split("");
      int count = 0;
      String target = "";
      for (int i = 0; i < character.length; i++) {
        if (character[i].equals(target)) {
          continue;
        }
        for (int j = 0; j < i; j++) {
          if (character[i].equals(character[j])) {
            target = character[i];
            count++;
            break;
          }
        }
      }
      return count;
    }
  }
  ```
  
  * 문자열을 쪼개서 target 문자 하나를 정해주고 target문자와 같은 문자가 있으면 count를 증가시키는 방향으로 풀면되겠다 생각해서 이중 for문으로 구현했다. 
  * 근데 그렇게 하면 이미 count 증가한 문자를 또 체크하는 상황이 발생할 수 있어서, `String target`에 저장하고, 바깥 for문에서 target문자가 나오면 continue로 해결했다.
    * 처음에 count증가 후 text에서 target문자들을 replace() 메소드로 제거하면 되지 않을까? 생각했는데 그러면 for문 index때문에 안될거 같아서 관뒀다.
  
* 입력 인자에 대문자가 있을 경우

  * 테스트 코드

  ```java
    @Test
    public void testWhenDifferentCaseShouldCaseInsensitive() {
      //Given
      String given = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
      //When
      int actual = CountingDuplicates.duplicateCount(given);
      //Then
      assertEquals(26, actual);
    }
  ```
  
  * 실제 코드 
  
  ```java
    public class CountingDuplicates {
    
      public static int duplicateCount(String text) {
        text = text.toLowerCase();
        String[] character = text.split("");
        int count = 0;
        String target = "";
        for (int i = 0; i < character.length; i++) {
          if (character[i].equals(target)) {
            continue;
          }
          for (int j = 0; j < i; j++) {
            if (character[i].equals(character[j])) {
              target = character[i];
              count++;
              break;
            }
          }
        }
        return count;
      }
    }
  ```
    * 단순히 `text = text.toLowerCase()`만 붙여줌
    * 이중 for문 사용하지않고 해결 할 수 있을까.. 고민하다가 그냥 제출했다.
    * Success (약 18분)
  
  
####  2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class CountingDuplicates {
  public static int duplicateCount(String text) {
    int ans = 0;
    text = text.toLowerCase();
    while (text.length() > 0) {
      String firstLetter = text.substring(0,1);
      text = text.substring(1);
      if (text.contains(firstLetter)) ans ++;
      text = text.replace(firstLetter, "");
    }
    return ans;
  }
}
```

* while문 하나로 해결했다. 


