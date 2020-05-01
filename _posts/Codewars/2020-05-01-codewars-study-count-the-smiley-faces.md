---
title: "Codewars 문제풀기 (05/01)"
excerpt: "Count the smiley faces!(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-01
---



# [Count the smiley faces!](https://www.codewars.com/kata/583203e6eb35d7980400002a/train/java)

* List\<String>을 인자로 받는다.
* list는 face이모티콘( :)  ;(  :-D 등)이다.
* list에 있는 smile face 이모티콘 count를 리턴한다
* 웃는 얼굴은
  * 눈은 :이나 ;이어야하고
  * 코는 -나 ~이되 없어도 된다.
  * 입은 D, )이어야한다.

```
countSmileys([':)', ';(', ';}', ':-D']);  👉 2
countSmileys([';D', ':-(', ':-)', ';~)']);     👉 3
countSmileys([';]', ':[', ';*', ':$', ';-D']); 👉 1
```

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 { :), :D, :-}, :-() } 일때 2를 리턴

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return 2 when input is {:), :D, :-}, :-()}")
    public void test1() {
      // Given: Set face array
      List<String> given = new ArrayList<>();
      given.add(":)");
      given.add(":D");
      given.add(":-}");
      given.add(":-()");
    
      // Then: Should return 2
      assertEquals(2, SmileFaces.countSmileys(given));
    }
    ```
    
    * When없이 Then으로 처리해도 괜찮다고 생각해서 없앴다.
    
  * 실제 코드
    
      ```java
      import java.util.List;
      
      public class SmileFaces {
      
        public static int countSmileys(List<String> arr) {
          int smileCount = 0;
          for (int i = 0; i < arr.size(); i++) {
            String face = arr.get(i);
            if (face.matches("^[:;][-~]?[)D]$")) {
              smileCount++;
            }
          }
          return smileCount;
        }
      }
      ```
      
      * 정규식을 자세히 아는건 아니지만 이정도는 알고 있어서 정규식으로 풀었다.

---

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class SmileFaces {
  
  public static int countSmileys(List<String> arr) {
    return (int)arr.stream().filter( x -> x.matches("[:;][-~]?[)D]")).count();
  }
}
```

* 스트림으로도 해결 할 수 있다.



* 정규식 간단 정리
  * ^ : 시작 (^The 👉 The로 시작)
  * & : 끝 (The$ 👉 The로 끝남) 
  * \* : 0개 이상 문자 (The* 👉 Th와 e가 0개 이상)
  * \+ : 1개 이상 문자 (The+ 👉 Th와 e가 1개 이상)
  * ? : 있거나 없거나 (The? 👉 Th와 e가 있거나 없거나)
  * {n} : n개의 문자 (The{2} 👉 Th와 e가 2개)
  * {n, } : n개 이상의 문자
  * {n, m} : n개 이상 m개 이하의 문자

