---
title: "Codewars 문제풀기 (03/26)"
excerpt: "Dubstep"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-26
---



## [Dubstep](https://www.codewars.com/kata/551dc350bf4e526099000ae5/train/java)

* "WUB"가 포함된 String을 인자로 받는다.
* "WUB"가 삭제된 문자열을 반환한다. WUB사이의 문자들은 space로 구분된다.
* songDecoder("WUBWUBABCWUB") --> WUB삭제 --> "ABC"
* songDecoder("RWUBWUBWUBLWUB") --> WUB삭제 --> "RL" --> R과 L사이는 space로 구분 --> "R L"

#### 1. Test를 만들었다.

* 입력 String이 "WUBWUBABCWUB" 일 때

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnABCWhenStringIsWUBWUBABCWUB() {
        //Given : WUBWUBABCWUB
        String given = "WUBWUBABCWUB";
        //When : Call SongDecoder
        String actual = Dubstep.songDecoder(given);
        //Then : Should return "ABC"
        assertEquals("ABC", actual);
      }
    ```
    
  * 실제 코드
  
  ```java
    public class Dubstep {
    
      public static String songDecoder(String song) {
        return song.replaceAll("WUB", "");
      }
    }
    ```
  
* 입력 String이 "RWUBWUBWUBLWUB"일 떄

  * 테스트 코드

    ```java
      @Test
      public void testShouldRAndLWhenStringIsRWUBWUBWUBLWUB() {
        //Given : RWUBWUBWUBLWUB
        String given = "RWUBWUBWUBLWUB";
        //When : Call SongDecoder
        String actual = Dubstep.songDecoder(given);
        //Then : Should return "R L"
        assertEquals("R L", actual);
      }
    ```
    
* 실제 코드
  
  ```java
    public class Dubstep {
    
      public static String songDecoder(String song) {
        String[] splitByWUB = song.split("WUB");
        StringBuilder result = new StringBuilder();
        int count = 0;
        for (int i = 0; i < splitByWUB.length; i++) {
          if (!splitByWUB[i].equals("") && count == 0) {
            result.append(splitByWUB[i]);
            count++;
          } else if (!splitByWUB[i].equals("") && count != 0) {
            result.append(" ").append(splitByWUB[i]);
          }
        }
        return result.toString();
      }
    }
    ```
  
    * 공백 처리를 어떻게 할 줄 몰라서 어쩔 수 없이  count를 선언해서 count가 0일 경우는 result문자열에 그냥 붙여주고, count가 0이 아닐경우 space + 문자열을 붙여주는것으로 해결했다.
  
  * Success(약 10분)


#### 2. 리팩토링

* 분명히 count를 두지않고 더 좋은 방법이 있을거 같아 고민해봤다. 스트림을 이용하면 될거같은데.. "WUB"로 split한 String 배열들을 stream에 넣고, join하면 되지않을까? 근데 그러면 String 배열에 ""문자열도 포함되어 공백이 늘어날텐데.. 그러면 filter()로 문자열이 ""이 아닌 애들만 join하면 되겠다. join할때 delimiter를 " "로 두면 되겠다!

  ```java
  public class Dubstep {
  
    public static String songDecoder(String song) {
      String result = Arrays.stream(song.split("WUB")).filter(s -> !s.equals("")).collect(Collectors.joining(" "));
  
      return result;
    }
  }
  ```
  
  * 테스트 돌려보니 통과했다. 그대로 제출했다.

#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Dubstep {
  public static String SongDecoder (String song)
  {
     return song.replaceAll("(WUB)+", " ").trim();
  }
}
```

* stream으로 해결하려고 고민했을 때, 비교적 해답이 빠르게 나왔다. 기분좋아서 제출했는데 더 쉬운 방법이 있었다..
* 우선 trim()메서드를 잘 몰랐다. 공백을 없애주는 메서드라고 알고있었는데, 문자열 앞 뒤로 있는 공백만 없애주는 것을 몰랐다.
* 정규표현식도 아직 잘 모르는것 같다 처음에 "(WUB)+"가 무슨 말인지 잘 몰라서 구글링 해보니 +가 붙으면 "WUB"가 1개 이상이면 조건을 만족한다는 뜻이다. 그래서 "WUB"나 "WUBWUBWUB"나 "WUBWUBWUBWUBWUBWUB" 모두 " "로 대체한다. 그러면 "RWUBWUBWUBLWUB"는 "R L "가 되는데 trim()메서드를 쓰면 'L'뒤의 space를 없애주므로 "R L"만 나오게 된다.



#### 모르는거 공부

* trim() 메서드

  * 문자열의 왼쪽 오른쪽 공백을 제거해준다. 문자열 가운데에 있는 공백은 제거하지 않는다.

    ```java
    //String.java
    public String trim() {
      int len = value.length;
      int st = 0;
      char[] val = value;    /* avoid getfield opcode */
    
      while ((st < len) && (val[st] <= ' ')) {
        st++;
      }
      while ((st < len) && (val[len - 1] <= ' ')) {
        len--;
      }
      return ((st > 0) || (len < value.length)) ? substring(st, len) : this;
    }
    ```

* 간단한 정규 표현식

  ![regular-expression]({{site.url}}/assets/images/2020-03-26-codewars-study-dubstep.assets/regular-expression.png)

