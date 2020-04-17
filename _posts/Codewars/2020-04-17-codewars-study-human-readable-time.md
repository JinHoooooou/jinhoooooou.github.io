---
title: "Codewars 문제풀기 (04/17)"
excerpt: "Human Readable Time(5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-17
---



## [Human Readable Time](https://www.codewars.com/kata/52685f7382004e774f0001f7/train/java)

* seconds를 인자로 받아 시 - 분 - 초로 리턴한다.
* makeReadable(0) 👉 "00:00:00"
* makeReadable(5) 👉 "00:00:05"
* makeReadable(60) 👉 "00:01:00"
* makeReadable(86399) 👉 "23:59:59"

#### 1. Test와 리팩토링

* 테스트 1 - 입력 인자가 음수라면 null을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnNullWhenInputIsNegativeInteger() {
      // Given: Set negative integer
      int given = -3;
      // When: Call makeReadable method
      String actual = HumanReadableTime.makeReadable(given);
      // Then: Should return null
      assertEquals(null, actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
        return null;
      }
    }
    ```

  * 리팩토링 - seconds가 0보다 크면 "" 작으면 null을 리턴하도록 변경

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
        return seconds > 0 ? "" : null;
      }
    }
    ```

* 테스트 2 - 입력 인자가 59일경우 00:00:59를 리턴한다.

  - 테스트 코드

    ```java
    @Test
    public void testShouldReturn59SecondsWhenInputIs59() {
      // Given: Set negative integer
      int given = 59;
      // When: Call makeReadable method
      String actual = HumanReadableTime.makeReadable(given);
      // Then: Should return "00:00:59"
      assertEquals("00:00:59", actual);
      }
    ```

  * 실제 코드

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
        return seconds > 0 ? "00:00:59" : null;
      }
    }
    ```
    
  * 리팩토링 - `String.format()`메서드를 이용해서 seconds를 출력하도록 변경

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
        return seconds > 0 ? String.format("00:00:%d",seconds) : null;
      }
    }
    ```

* 테스트 3 - 입력 인자가 100일경우 00:01:40을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn1MinuteAnd40SecondsWhenInputIs100() {
      // Given: Set negative integer
      int given = 100;
      // When: Call makeReadable method
      String actual = HumanReadableTime.makeReadable(given);
      // Then: Should return "00:01:40"
      assertEquals("00:01:40", actual);
      }
    ```

  * 실제 코드

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
        int minutesTen = 0;
        int minutesOne = 0;
        int secondsOne = seconds;
        int secondsTen = 0;
          
        while(seconds >= 10) {
          secondsTen++;
          if(secondsTen == 6) {
          secondsTen = 0;
            minutesOne++;
          }
          if(minutesOne == 10) {
            minutesOne = 0;
            minutesTen++;
          }
          secondsOne -= 10;
        }
        return seconds > 0 ? String.format("00:%d%d:%d%d",minutesTen, minutesOne, secondsTen, secondsOne) : null;
      }
    }
    ```
    
    * seconds나 minutes이 10보다 작으면 "01, 06" 이런식으로 출력되는게아니라 "1, 6" 이런식으로 출력된다. 그래서 일단 1의 자리와 10의자리 나누어서 계산했다.
    
  * 리팩토링 - 초를 3600으로 나눈 몫이 시(hour)이고, 나머지가 시를 제외한 초이다. 시를 제외한 초에서 60으로 나누면 분(minute)이고 나머지가 시,분을 제외한 순수한 초(second)이다.

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
    
        int intMinute = seconds % 3600 / 60;
        int intSecond = seconds % 3600 % 60;
    
        String minute = intMinute < 10 ? "0" + intMinute : "" + intMinute;
        String second = intSecond < 10 ? "0" + intSecond : "" + intSecond;
    
        return seconds > 0 ? String.format("00:%s:%s", minute, second) : null;
      }
    }
    ```

* 테스트 4 - 입력 인자가 86399이면 "23:59:59"를 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn23HoursAnd59MinutesAnd59SecondsWhenInputIs86399() {
      // Given: Set negative integer
      int given = 86399;
      // When: Call makeReadable method
      String actual = HumanReadableTime.makeReadable(given);
      // Then: Should return "23:59:59"
      assertEquals("23:59:59", actual);
    }
    ```

  * 실제 코드

    ```java
    public class HumanReadableTime {
    
      public static String makeReadable(int seconds) {
    
        int intHour = seconds / 3600;
        int intMinute = seconds % 3600 / 60;
        int intSecond = seconds % 3600 % 60;
    
        String hour = intHour < 10 ? "0" + intHour : "" + intHour;
        String minute = intMinute < 10 ? "0" + intMinute : "" + intMinute;
        String second = intSecond < 10 ? "0" + intSecond : "" + intSecond;
    
        return seconds > 0 ? String.format("%s:%s:%s",hour, minute, second) : null;
    }
    }
    
    ```
    

* 제출 (약 15분)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class HumanReadableTime {
  public static String makeReadable(int seconds) {
    return String.format("%02d:%02d:%02d", seconds / 3600, (seconds / 60) % 60, seconds % 60);
  }
}
```

* 00:00:00 포맷으로 출력할 때 "%02d"를 생각하지 못하고 String으로 만들어 출력했다. ㅜㅜ 이걸 생각 못하는... ㅂㄷㅂㄷ
