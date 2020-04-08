---
title: "Codewars 문제풀기 (04/08)"
excerpt: "Credit Card Mask(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-08
---



## [Credit Card Mask](https://www.codewars.com/kata/5412509bd436bd33920011bc/train/java)

* String을 인수로 받는다.
* 마지막 4문자를 남기고 "#"문자로 치환한 문자열을 리턴한다.
* 입력 인수 길이가 4 이하라면 인수 그대로 리턴한다.
* maskify("4556364607935616") --> "############5616"
* maskify("Nananananananananananananananana Batman!") --> "####################################man!"
* maskify("1") --> "1"
* maskify("") --> ""

#### 1. Test와 리팩토링

* 입력 String 인수의 길이가 4이하라면 입력 String 그대로 리턴해야한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnOriginalStringWhenInputLengthLessThan4() {
        // Given: Set string length less than 4
        String given = "asd";
        // When: Call maskify method
        String actual = Maskify.maskify(given);
        // Then: Should return input string
        assertEquals(given, actual);
      }
    ```
    
  * 실제 코드

    ```java
    public class Maskify {
        
      public static String maskify(String str) {
        return str.length() < 5 ? str : "";
      }
    }
    ```
    
  * 첫번째 테스트를 만족하는 코드만 작성했다.

* 입력 String 인수 길이가 4보다 크면, 마지막 4문자만 남기고 모든 문자를 '#'로 치환한다.

  * 테스트 코드

    ```java
    
      @Test
      public void testShouldReplaceAllCharacterToHashTagExceptLast4Characters() {
        // Given: Set string length more than 4
        String given = "mynameisleejinho";
        // When: Call maskify method
        String actual = Maskify.maskify(given);
        // Then: Should return Replace # except last 4 string
        assertEquals("############inho", actual);
      }
    ```
    
  * 실제 코드

    ```java
    public class Maskify {
    
      public static String maskify(String str) {
        if (str.length() < 5) {
          return str;
        }
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < str.length() - 4; i++) {
          result.append("#");
        }
        result.append(str.substring(str.length() - 4));
        return result.toString();
      }
    }
    ```

    * StringBuilder를 선언하여 입력 String length - 4만큼 '#'을 붙이고, 뒤에 마지막 문자4개를 붙여 반환했다.

  * 리팩토링

    1. 우선 매직넘버를 상수로 추출 할 수 있을것 같다.

       ```java
       public class Maskify {
       
           public static final int UNMASK_STRING_LENGTH = 4;
       
         public static String maskify(String str) {
           if (str.length() <= UNMASK_STRING_LENGTH) {
             return str;
           }
           StringBuilder result = new StringBuilder();
           for (int i = 0; i < str.length() - UNMASK_STRING_LENGTH; i++) {
             result.append("#");
           }
           result.append(str.substring(str.length() - UNMASK_STRING_LENGTH));
           return result.toString();
         }
       }
       ```

       * 매직넘버 4를 UNMASK_STRING_LENGTH로 추출했다.

    2. for문을 굳이 쓸 필요가 있을까? 그리고 StringBuilder를 쓰지않아도 될 것 같다.

       ```java
       public class Maskify {
       
           public static final int UNMASK_STRING_LENGTH = 4;
       
         public static String maskify(String str) {
           if (str.length() <= UNMASK_STRING_LENGTH) {
             return str;
           }
           String unMaskString = str.substring(str.length() - UNMASK_STRING_LENGTH);
           String maskString = str.substring(0, str.length() - UNMASK_STRING_LENGTH)
               .replaceAll(".", "#");
           return maskString + unMaskString;
         }
       }
       ```

       * 구글링을해보니 모든 문자에 대한 정규표현식이 "."이었다. 그래서 replaceAll()메서드를 이용하여 모든 문자들을 #문자로 바꾸었다.
       * mask, unmask string을 지역변수로 따로 만들었다.
       * 더 리팩토링할 요소가 없다고 판단하여 제출했다.(리팩토링 제외 문제풀기만 15분정도 소요)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Maskify {
    public static String maskify(String str) {
        if (str.length() <= 4) return str;
        String result = "";
        for (int i = 0; i < str.length()-4; i++) {
            result += "#";
        }
        return result + str.substring(str.length()-4);
    }
}
```

* 처음에 작성한 코드와 크게 다르지 않다 다른점은 StringBuilder를 사용하지않고 String을 사용한것. 

  

#### 3. 알게된 것

* 모든 문자에 대한 정규표현식 --> "."
* 모든 문자를 '!'문자로 바꾸려면 --> replaceAll(".", "!");
