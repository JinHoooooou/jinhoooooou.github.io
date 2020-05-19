---
title: "Codewars 문제풀기 (05/19)"
excerpt: "Human readable duration format (4kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-19
---



# [Human readable duration format](https://www.codewars.com/kata/52742f58faf5485cae000b9a/train/java)

* int를 입력으로 받는다.

* 정수를 time format으로 변경하여 String으로 리턴한다.

* year, day, hour, minute, second으로 표현한다.

  ``` 
  TimeFormatter.formatDuration(1) 👉 1 second
  TimeFormatter.formatDuration(62) 👉 1 minute and 2 seconds
  TimeFormatter.formatDuration(3722) 👉 1 hour, 2 minutes and 2 seconds
  TimeFormatter.formatDuration(7202) 👉 2 hours and 2 seconds 
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 1이면 "1 second"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/ea0f826c7d0004a108a8b4655301807cf6ad3b5d)

    ```java
    @Test
    @DisplayName("test should return 1 second when input 1")
    public void test1() {
      // Then: Should return 1 second
      assertEquals("1 second", TimeFormatter.formatDuration(1));
    }
    ```
    
    
    
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/b38b18698d59dafa9b51c89896c3f4120585c446)
  
    ```java
  public class TimeFormatter {
    
    public static String formatDuration(int seconds) {
        return seconds + " second";
    }
    }
    ```
  
* ### 테스트 2 - 입력이 62이면 "1 minute and 2 seconds"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/3f1e124d0f8b40af65396b001958da164a25016a)

    ```java
    @Test
    @DisplayName("test should return 1 minute and 2 seconds second when input 62")
    public void test2() {
      // Then: Should return 1 minute and 2 seconds
      assertEquals("1 minute and 2 seconds", TimeFormatter.formatDuration(62));
    }
    ```
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/68828b8213172988fb49fbe62600649444f5e455)

    ```java
    public class TimeFormatter {
    
      public static String formatDuration(int seconds) {
        String result = "";
        if (seconds >= 60) {
          if (seconds / 60 > 1) {
            result = seconds / 60 + " minutes and ";
          } else {
            result = seconds / 60 + " minute and ";
          }
        }
        if (seconds % 60 > 1) {
          result = result + seconds % 60 + " seconds";
        } else {
          result = result + seconds % 60 + " second";
        }
    
        return result;
      }
    }
    ```

    * 값이 1보다 크면 format에 s를 붙여줘야하기 때문에 if문으로 처리했다.

  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/d8f2cd58b8fd89461e9aa0892e17a2087fbc1561)

    ```java
    public class TimeFormatter {
    
      public static String formatDuration(int seconds) {
        String result = "";
        int second = seconds % 60;
        int minute = seconds / 60;
        if (minute > 0) {
          result = minute + (minute > 1 ? " minutes and " : " minute and ");
        }
        result += second + (second > 1 ? " seconds" : " second");
    
        return result;
      }
    }
    ```
    
    * 3항 연산자로 더 짧게 처리했다.

* ### 테스트 3 - 입력이 60이면 "2 minutes"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/085611bd180093d802b14bfc59ea7923760c2310)

    ```java
    @Test
    @DisplayName("test should return 2 minutes when input 120")
    public void test3() {
      // Then: Should return 2 minutes
      assertEquals("2 minutes", TimeFormatter.formatDuration(120));
    }
    ```
    
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/6d1d561e5d1b4461c016f7989e84527f0799aa49)

    ```java
    public class TimeFormatter {
      public static String formatDuration(int seconds) {
        String result = "";
        int second = seconds % 60;
        int minute = seconds / 60;
        if (second > 0) {
          result = second + (second > 1 ? " seconds" : " second");
        }
        if (minute > 0) {
          if (second > 0) {
            result = " and " + result;
          }
          result = minute + (minute > 1 ? " minutes" : " minute") + result;
        }
    
        return result;
      }
    }
    ```
    
    * "2 minutes" 이후에 second가 없으면 "and"를 붙이면 안된다. 그래서 second가 0보다 클 때만 "and"를 붙이도록 했다.

* ### 테스트 4 - 입력이 3600이면 "1 hour"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/12319e308b2fa88117553f847ca84d8e533a6caf)

    ```java
    @Test
    @DisplayName("test should return 1 hour when input 3600")
    public void test4() {
      // Then: Should return 1 hour
      assertEquals("1 hour", TimeFormatter.formatDuration(3600));
    }
    ```
    
    
    
* [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/75afba540d649d46295160ab8c65dcd30ec6e4c3)
  
  ```java
    public class TimeFormatter {
    
      public static String formatDuration(int seconds) {
        String result = "";
        int hour = seconds / 3600;
        int minute = seconds % 3600 / 60;
        int second = seconds % 60;
    
        if (second > 0) {
          result = second + (second > 1 ? " seconds" : " second");
        }
        if (minute > 0) {
          if (second > 0) {
            result = " and " + result;
          }
          result = minute + (minute > 1 ? " minutes" : " minute") + result;
        }
      if (hour > 0) {
        if (minute > 0 || second > 0) {
            result = " and" + result;
          }
          result = hour + (hour > 1 ? " hours" : " hour") + result;
        }
    
        return result;
      }
    }
    ```
    
    * 이 test만 봤을 때 "1 hour"가 리턴되도록 구현했다.

* ### 테스트 5 - 입력이 3662일때 "1 hour, 1 minute and 2 seconds"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/a1000134ce5b8b0f8b91e1a4340c7eca9a8b5d22)

    ```java
    @Test
    @DisplayName("test should return 1 hour, 1 minute and 2 seconds when input 3662")
    public void test5() {
      // Then: Should return 1 hour, 1 minute and 2 seconds
      assertEquals("1 hour, 1 minute and 2 seconds", TimeFormatter.formatDuration(3662));
    }
    ```

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/29618c5627aac7ea1ccf37ea5e794b64ce10b6dd)

    ```java
    public class TimeFormatter {
      public static String formatDuration(int seconds) {
        String result = "";
        int hour = seconds / 3600;
        int minute = seconds % 3600 / 60;
        int second = seconds % 60;
        if (second > 0) {
          result = second + (second > 1 ? " seconds" : " second");
        }
        if (minute > 0) {
          if (second > 0) {
            result = " and " + result;
          }
          result = minute + (minute > 1 ? " minutes" : " minute") + result;
        }
        if (hour > 0) {
          if (minute > 0 || second > 0) {
            result = ", " + result;
          }
          result = hour + (hour > 1 ? " hours" : " hour") + result;
        }
        return result;
      }
    }
    ```

    * "and"가 붙는게 아니라 ","가 붙기 때문에 수정했다.

* ### 테스트 6 - 입력이 15731080이면 "182 days, 1 hour, 44 minutes and 40 seconds"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/3ce6e5e3ec53d4a92fec238ac7b96a53a2b580d3)

    ```java
    @Test
    @DisplayName("test should return 182 days, 1 hour, 44 minutes and 40 seconds when input 15731080")
    public void test6() {
      // Then: Should return 182 days, 1 hour, 44 minutes and 40 seconds
      assertEquals("182 days, 1 hour, 44 minutes and 40 seconds",
           TimeFormatter.formatDuration(15731080));
    }
    ```

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/124bd4e8e251d7db4b94c9dcc72e384ad76e2c60)

    ```java
    public class TimeFormatter {
    
      public static String formatDuration(int seconds) {
        String result = "";
        int day = seconds / (3600 * 24);
        int hour = seconds % (3600 * 24) / 3600;
        int minute = seconds % 3600 / 60;
        int second = seconds % 60;
    
        if (second > 0) {
          result = second + (second > 1 ? " seconds" : " second");
        }
        if (minute > 0) {
          if (second > 0) {
            result = " and " + result;
          }
          result = minute + (minute > 1 ? " minutes" : " minute") + result;
        }
        if (hour > 0) {
          if (minute > 0 || second > 0) {
            result = ", " + result;
          }
          result = hour + (hour > 1 ? " hours" : " hour") + result;
        }
    
        if (day > 0) {
          if (hour > 0 || minute > 0 || second > 0) {
            result = ", " + result;
          }
          result = day + (day > 1 ? " days" : " day") + result;
        }
    
        return result;
      }
    }
    ```

    * 단순히 day의 경우만 추가했다.

* ### 테스트 7 - 입력이 0이면 "now"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/a540a79ee558f3af920e85f432a7e087faed812e)

    ```java
    @Test
    @DisplayName("test should return now when input 0")
    public void test7() {
      // Then: Should return now
      assertEquals("now", TimeFormatter.formatDuration(0));
    }
    ```

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/4d90478129432c39fc30def35153640359571fe0)

    ```java
    public class TimeFormatter {
    
      public static String formatDuration(int seconds) {
        if (seconds == 0) {
          return "now";
        }
        ...
      }
    }
    ```

    * seconds가 0일 때의 경우를 추가했다.

  * [리팩토링1](https://github.com/JinHoooooou/codeWarsChallenge/commit/95a79ad09d95a0f7479fb683c70a92e2d52c3c90)

    ```java
    import java.util.ArrayList;
    import java.util.List;
    
    public class TimeFormatter {
    
      public static String formatDuration(int seconds) {
        if (seconds == 0) {
          return "now";
        }
        String result = "";
        List<String> format = new ArrayList<>();
        int year = seconds / (3600 * 24 * 365);
        int day = seconds % (3600 * 24 * 365) / (3600 * 24);
        int hour = seconds % (3600 * 24) / 3600;
        int minute = seconds % 3600 / 60;
        int second = seconds % 60;
        if (year > 0) {
          format.add(year + (year > 1 ? " years" : " year"));
        }
        if (day > 0) {
          format.add(day + (day > 1 ? " days" : " day"));
        }
        if (hour > 0) {
          format.add(hour + (hour > 1 ? " hours" : " hour"));
        }
        if (minute > 0) {
          format.add(minute + (minute > 1 ? " minutes" : " minute"));
        }
        if (second > 0) {
          format.add(second + (second > 1 ? " seconds" : " second"));
        }
        if (format.size() > 1) {
          result = String.join(", ", format);
          String frontComponent = result.substring(0, result.lastIndexOf(","));
          String lastComponent = result.substring(result.lastIndexOf(",") + 1);
          result = frontComponent + " and" + lastComponent;
        } else {
          result = String.join("", format);
        }
    
        return result;
      }
    }
    ```

    * 문제를 잘 못 봤다. "years, days, hours, minutes and second" format으로 리턴해야한다.  "years and seconds" 이런식으로.. 그래서 생각한 방법이 각 format에 해당하는 값이 1 이상이면 List에 저장한 뒤 `String.join()` 메서드로 ", "를 붙여서 이어준다. 그리고 마지막 ", "를 "and "로 수정한 뒤 리턴하도록 구현했다.

  * [리팩토링 2](https://github.com/JinHoooooou/codeWarsChallenge/commit/16206e35b059d471c44796b7408d0ba489c4a644)

    ```java
    public class TimeFormatter {
    
      private static int SECOND_PER_MINUTE = 60;
      private static int SECOND_PER_HOUR = 60 * SECOND_PER_MINUTE;
      private static int SECOND_PER_DAY = 24 * SECOND_PER_HOUR;
      private static int SECOND_PER_YEAR = 365 * SECOND_PER_DAY;
      
      public static String formatDuration(int seconds) {
        if (seconds == 0) {
          return "now";
        }
        int year = seconds / SECOND_PER_YEAR;
        seconds = seconds % SECOND_PER_YEAR;
        int day = seconds / SECOND_PER_DAY;
        seconds = seconds % SECOND_PER_DAY;
        int hour = seconds / SECOND_PER_HOUR;
        seconds = seconds % SECOND_PER_HOUR;
        int minute = seconds / SECOND_PER_MINUTE;
        int second = seconds % SECOND_PER_MINUTE;
        ...
      }
    }
    ```

    * 매직 넘버를 상수로 변환했다.

  * [리팩토링 3](https://github.com/JinHoooooou/codeWarsChallenge/commit/1147497cfc78f15bcdf55c596527233026435cf0)

    ```java
    public class TimeFormatter {
      private static int SECOND_PER_MINUTE = 60;
      private static int SECOND_PER_HOUR = 60 * SECOND_PER_MINUTE;
      private static int SECOND_PER_DAY = 24 * SECOND_PER_HOUR;
      private static int SECOND_PER_YEAR = 365 * SECOND_PER_DAY;
      public static String formatDuration(int seconds) {
        ...
    
        List<String> format = new ArrayList<>();
        if (year > 0) {
          format.add(year + (year > 1 ? " years" : " year"));
        }
        if (day > 0) {
          format.add(day + (day > 1 ? " days" : " day"));
        }
        if (hour > 0) {
          format.add(hour + (hour > 1 ? " hours" : " hour"));
        }
        if (minute > 0) {
          format.add(minute + (minute > 1 ? " minutes" : " minute"));
        }
        if (second > 0) {
          format.add(second + (second > 1 ? " seconds" : " second"));
        }
        return makeStringFormat(format);
      }
    
      private static String makeStringFormat(List<String> format) {
        String result = "";
        if (format.size() > 1) {
          result = String.join(", ", format);
          String frontComponent = result.substring(0, result.lastIndexOf(","));
          String lastComponent = result.substring(result.lastIndexOf(",") + 1);
          result = frontComponent + " and" + lastComponent;
        } else {
          result = String.join("", format);
        }
        return result;
      }
    }
    ```

    * ", "를 "and "로 수정하는 부분을 따로 메서드 추출했다.

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Arrays;
import java.util.stream.*;
public class TimeFormatter {    
    public static String formatDuration(int seconds) {            
        return seconds == 0 ? "now" : 
                Arrays.stream(
                  new String[]{
                       formatTime("year",  (seconds / 31536000)),
                       formatTime("day",   (seconds / 86400)%365),
                       formatTime("hour",  (seconds / 3600)%24),
                       formatTime("minute",(seconds / 60)%60),
                       formatTime("second",(seconds%3600)%60)})
                      .filter(e->e!="")
                      .collect(Collectors.joining(", "))
                      .replaceAll(", (?!.+,)", " and ");
    }
    public static String formatTime(String s, int time){
      return time==0 ? "" : Integer.toString(time)+ " " + s + (time==1?"" : "s");
    }
}
```

* `formatTime`이라는 메서드를 만들어서 값이 String 배열에 저장 후 ", "를 붙여주면서 join한다. 그리고 마지막 ", "를 "and "로 바꾸는 것을 정규식을 이용했다.
* 처음으로 4Kyu 문제를 풀어봤는데 4Kyu중에 쉬운 문제긴 했지만 시간이 좀 걸렸다.
