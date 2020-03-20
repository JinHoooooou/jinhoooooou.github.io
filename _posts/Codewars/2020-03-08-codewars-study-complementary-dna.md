---
title: "Codewars 문제풀기 (03/08)"
excerpt: "Complementary DNA"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-08
---



## [Complementary DNA](https://www.codewars.com/kata/554e4a2f232cdd87d9000038)

* A, T, G, C로 이루어진 문자열을 인자로 받는다
* A는 T, T는 A, G는 C, C는 G로 바꿔 반환한다.
* A, T, G, C외의 문자는 없다고 가정한다.


#### 1. Test를 만들었다

* A로만 이루어진 문자열

  replaceAll메소드로 A를 T로 변환해서 리턴하자

  ```java
  public class DnaStrand {
  
    public static String makeComplement(String dna) {
        return dna.replaceAll("A", "T");
    }
  }
  ```

* A와 C로만 이루어진 문자열

  마찬가지로 A를 T로 C를 G로 변환해서 리턴한다.

  ```java
  public class DnaStrand {
  
    public static String makeComplement(String dna) {
        return dna.replaceAll("A", "T").replaceAll("C", "G");
    }
  }
  ```
  
* A, T, C, G 모두 있는 문자열

  이전처럼 `replaceAll("A", "T").replaceAll("T","A")`를 쓰면 A가 T로 변환되고 변환된 T까지 A로 변환되기 때문에 결과값이 A만 남게된다. 그래서 다른방법 선택

  ```java
  public class DnaStrand {
  
    public static String makeComplement(String dna) {
  
      StringBuilder result = new StringBuilder();
      for (int i = 0; i < dna.length(); i++) {
        switch (dna.charAt(i)) {
          case 'A':
            result.append("T");
            break;
          case 'T':
            result.append("A");
            break;
          case 'C':
            result.append("G");
            break;
          case 'G':
            result.append("C");
            break;
          default:
            break;
        }
      }
      return result.toString();
    }
  }
  
  ```

  리팩토링 없이 바로 제출

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class DnaStrand {
  public static String makeComplement(String dna) {
    char[] chars = dna.toCharArray();
    for(int i = 0; i < chars.length; i++) {
      chars[i] = getComplement(chars[i]);
    }
    
    return new String(chars);
  }
  
  private static char getComplement(char c) {
    switch(c) {
      case 'A': return 'T';
      case 'T': return 'A';
      case 'C': return 'G';
      case 'G': return 'C';
    }
    return c;
  }
}
```

* 로직 자체는 같지만, switch문을 통해 변환하는 부분을 메소드로 추출하여 더 깔끔하게 표현했다.

Best Practice 두번째로 많이 받은 코드

```java
public class DnaStrand {
  public static String makeComplement(String dna) {
    dna = dna.replaceAll("A","Z");
    dna = dna.replaceAll("T","A");
    dna = dna.replaceAll("Z","T");
    dna = dna.replaceAll("C","Z");
    dna = dna.replaceAll("G","C");
    dna = dna.replaceAll("Z","G");
    return dna;
  }
}
```

* 흔히 swap 메소드를 구현하는것처럼 "Z"라는 임시 문자열로 변환 후 "Z"를 다시 "T", "G"로 변환하여 풀었다. 개인적으로 이 방법이 더 좋다고 생각한다.


#### 4. 궁금한거 공부

* 없음
