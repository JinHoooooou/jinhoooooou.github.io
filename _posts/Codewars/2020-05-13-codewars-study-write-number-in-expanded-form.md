---
title: "Codewars 문제풀기 (05/13)"
excerpt: "Write Number in Expanded Form (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-13
---



# [Write Number in Expanded Form](https://www.codewars.com/kata/5842df8ccbd22792a4000245/train/java)

* int를 입력으로 받는다.

* 입력 정수를 a + b + c + d..형태의 String으로 리턴한다.

  ``` 
  Kata.expandedForm(12) 👉 10 + 2
  Kata.expandedForm(70304) 👉 70000 + 300 + 4
  Kata.expandedForm(90000) 👉 90000
  ```

  


## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 12일 때 "10 + 2" 를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/d8b33166aed3c0dec365212342904bfb0e603c1b)

    ```java
    @Test
    @DisplayName("test should return 10 + 2 when input is 12")
    public void test1() {
      // Then: should return 10 + 2
      assertEquals("10 + 2", Kata.expandedForm(12));
    }
    ```
    
    


  - [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/6bca0a5557074d4d61e6395af205223338f92fa5)

    ```java
    public class Kata {
    
      public static String expandedForm(int num) {
        int[] numbers = new int[(int) Math.log10(num) + 1];
        for (int i = 0; i < numbers.length; i++) {
          numbers[i] = num % 10;
          num /= 10;
        }
        String result = "";
        for (int i = numbers.length - 1; i >= 0; i--) {
          result += "" + (numbers[i] * (int) Math.pow(10, i));
          if (i != 0) {
            result += " + ";
          }
        }
        return result;
      }
    }
    ```
    
    * 배열에 각 digit을 저장하고 배열 index를 이용해서 해결했다.

* ### 테스트 2 - 입력이 70304일 때 "70000 + 300 + 4"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/f809e8f3f58af895a889e36679e22d96a70f23e6)

    ```java
    @Test
    @DisplayName("test should return 70000 + 300 + 4 when input is 70304")
    public void test2() {
      // Then: should return 70000 + 300 + 4
      assertEquals("70000 + 300 + 4", Kata.expandedForm(70304));
    }
    ```
    
    
    
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/25f100c809136e671f1f3b93e885abc9258bf57e)

    ```java
    public class Kata {
    
      public static String expandedForm(int num) {
        ...
        String result = "";
        for (int i = numbers.length - 1; i >= 0; i--) {
          if (numbers[i] != 0) {
            result += ""+ (numbers[i] * (int)Math.pow(10, i));
            if(i != 0) {
              result += " + ";
            }
          }
        return result;
      }
    }
    ```
    
    * 배열의 element가 0이 아닌 경우에만 String을 붙여주는 것으로 수정했다. 

* ### 테스트 3 - 입력이 90000일 때 "90000"을 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/815ae4e20a6fea6fb51c48e867077f53ae5c2bad)

    ```java
    @Test
    @DisplayName("test should return 90000 when input is 90000")
    public void test3() {
      // Then: should return 90000
      assertEquals("90000", Kata.expandedForm(90000));
    }
    ```
    
    
    
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/f60bd578daf94880517160a119935a445667470d)

    ```java
    public class Kata {
    
      public static String expandedForm(int num) {
        ...
        String result = "";
        for (int i = numbers.length - 1; i >= 0; i--) {
          if (numbers[i] != 0) {
            result += "" + (numbers[i] * (int) Math.pow(10, i)) + " + ";
          }
      }
        return result.substring(0, result.length() - 3);
      }
    }
    ```
    
    * 90000 같은 경우 + 가 필요없는데 +가 붙는다. 그래서 for문을 돌 때마다 +를 붙여주고 마지막에 붙은 +만 제거했다.
    
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/2854e6e7e7f8ad51a7e65dac163511f13fc9a650)

    ```java
    public class Kata {
    
      public static String expandedForm(int num) {
        int mulTimes = (int) Math.log10(num);
        for (int i = mulTimes; i >= 0; i--) {
          int pow = (int) Math.pow(10, i);
          int number = (num / pow) * pow;
          if (number != 0) {
            result += "" + number + " + ";
          }
          num = num % pow;
        }
        return result.substring(0, result.length() - 3);
      }
    }
    ```

    * 배열에 저장할 필요가 없다고 생각해서 배열을 지웠다.

  


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Kata
{
    public static String expandedForm(int num)
    {
        String outs = "";
        for (int i = 10; i < num; i *= 10) {
            int rem = num % i;
            outs = (rem > 0) ? " + " + rem + outs : outs;
            num -= rem;
        }
        outs = num + outs;
        
        return outs;
    }
}
```

* 나는 " + " 를 뒤에 붙여주고 마지막에 제거 했다면, 이 코드는 앞부분에 붙여줬다. 그러면 1의 자리부터 시작하고 마지막에 가장 큰 수만 더해주면된다. 이 방법이 더 좋은것 같다.. 왜 이런생각을 못하냐 ㅂㄷㅂㄷ

