---
title: "Codewars 문제풀기 (03/30)"
excerpt: "Your order, please"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-30
---



## [Your order, please](https://www.codewars.com/kata/55c45be3b2079eccff00010f/train/java)

* String을 인자로 받는다.
* 문자열의 각 단어들은 1~9사이의 연속된 정수를 포함한다.
* 이 숫자들은 단어의 순서를 나타낸다.
* 순서대로 재배열하여 출력한다.
* 숫자가 없다면 출력 문자열에 포함하지 않는다.
* order("is2 Thi1s T4est 3a")  -->  "Thi1s is2 3a T4est" 
* order("4of Fo1r pe6ople g3ood th5e the2")  -->  "Fo1r the2 g3ood 4of th5e pe6ople"
* order("") --> ""
* order("Empty input should return empty string") --> ""

#### 1. Test를 만들었다.

* 입력 String에 number가 포함되어있지 않아 순서가 없을 때 ""(empty string)을 출력한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnEmptyStringWhenInputIsEmptyString() {
        // Given: Set string number empty
        String given = "Empty input should return empty string";
        // When: Call order method
        String actual = Order.order(given);
        // Then: Should return empty string
        assertEquals("", actual);
      }
    ```
    
  * 실제 코드
  
    ```java
    public class Order {
    
      public static String order(String words) {
        if (!words.matches("[0-9]")) {
          return "";
        }
        return null;
    }
    ```
  
    * 처음에 matches() 메서드를 사용할 때, `matches("[0-9]")`썼는데 이게 잘못된 것인지 몰랐다.
  
* 입력 String에 number가 포함되어 있고, (2-1-4-3) 순서 일 때, (1-2-3-4) 순서로 출력한다.

  * 테스트 코드

    ```java
      @Test
      public void testShouldReturnNumberOrderIs1234WhenInputOrder2143() {
        // Given: Set string 2-1-4-3 order
        String given = "is2 Thi1s T4est 3a";
        // When: Call order method
        String actual = Order.order(given);
        // Then: Should return 1-2-3-4 order string
        assertEquals("Thi1s is2 3a T4est", actual);
      }
    ```
    
  * 실제 코드
    
    ```java
    public class Order {
    
      public static String order(String words) {
        if (!words.matches(".*[0-9].*")) {
          return "";
        }
        String[] split = words.split(" ");
        String[] result = new String[split.length];
        for(int i = 0; i<result.length; i++) {
            result[i] = "";
        }
        for(String eachWord : split) {
          if (eachword.contains("1")) {
            result[0] = eachWord;
          } else if (eachWord.contains("2")) {
            result[1] = eachWord;
          } else if (eachWord.contains("3")) {
            result[2] = eachWord;
          } else if (eachWord.contains("4")) {
            result[3] = eachWord;
          } else if (eachWord.contaions("5")) {
            result[4] = eachWord;
          } else if (eachWord.contaions("6")) {
            result[5] = eachWord;
          } else if (eachWord.contaions("7")) {
            result[6] = eachWord;
          } else if (eachWord.contaions("8")) {
            result[7] = eachWord;
          } else if (eachWord.contaions("9")) {
            result[8] = eachWord;
          }
        }
        return String.join(" ", result).trim();
    }
    ```
    
    * 바로 생각나는 답안이 일단 코드가 매우 더럽지만, 각 문자를 space(" ")로 쪼개서 1~9의 숫자가 포함되어 있을때 result라는 String 배열에 넣어서 join하는 방향으로 해결했다.
    * 처음에 문자열에 숫자가 포함되어있는지 확인하는 부분에서 처음 테스트가 틀렸다는것을 알고 찾아보니 `.*[0-9].*`이 숫자가 포함되어있는지 확인하는 정규식인것을 알게 되었다.
    * 일단 제출하긴 했는데 코드가 매우 더러워서 리팩토링을 시작했다.


#### 2. 리팩토링

* if / else문 말고 Stream써서 해결할 수 있을 것 같아서 코드를 좀 수정했다.

  ```java
  import java.util.Arrays;
  import java.util.stream.Collectors;
  public class Order {
  
    public static String order(String words) {
      String[] list = new String[10];
      Arrays.fill(list, "");
  
      for (int i = 1; i < 10; i++) {
        int finalI = i;
        String temp = Arrays.stream(words.split(" ")).filter(s -> s.contains(String.valueOf(finalI)))
            .collect(Collectors.joining());
        list[i] = temp;
      }
  
      return String.join(" ", list).trim();
    }
  ```
  
  * 이렇게 하면 문자열에 숫자가 없는지 따로 체크할 필요가 없기 때문에 삭제했다.
  * 제출하니까 통과하긴 했는데 만약에 연속된 숫자가 아니면 공백이 늘어나서 테스트 실패할것 같은데.. 라는 생각이 들었다. 일단 String에 포함된 1~9사이의 정수들은 연속된 숫자라고 했으니 상관없긴 했지만 좀 더 다른방법으로 수정할 수 있지 않을까 고민해봤다.
  
  ```java
  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.List;
  import java.util.stream.Collectors;
  
  public class Order {
  
    public static String order(String words) {
      List<String> list = new ArrayList<String>();
  
      for (int i = 0; i < 10; i++) {
        int finalI = i;
        String result = Arrays.stream(words.split(" ")).filter(s -> s.contains(String.valueOf(finalI)))
            .collect(Collectors.joining());
        if (!result.equals("")) {
          list.add(result);
        }
      }
  
      return String.join(" ", list).trim();
    }
  }
  ```
  
  * 쪼갠 문자열에서 1~9가 포함되어 있다면 해당 문자열을 temp에 저장하고, 없다면 ""를 저장한다. 그래서 result가 ""가 아니라면 List에 순서대로 저장한다. List에 순서대로 저장하면 순서 숫자가 연속되어있지 않아도 공백이 늘어나지않아 괜찮다.
  * 이대로 제출했다.

#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Arrays;
import java.util.Comparator;

public class Order {
  public static String order(String words) {
    return Arrays.stream(words.split(" "))
      .sorted(Comparator.comparing(s -> Integer.valueOf(s.replaceAll("\\D", ""))))
      .reduce((a, b) -> a + " " + b).get();
  }
}
```

* reduce() 전까지는 이해가 간다. 숫자외의 문자들을 전부 지우고, 남은 숫자 순서대로 정렬을 하는 것이다. 그런데 reduce()는 이해가 잘 안되었다. 직관적으로 보면 순서대로 정렬한 문자열을 붙여나가는 것 같은데.. reduce메서드를 잘 몰라서 모르겠다.

두번째로 많이 받은 코드

```java
public class Order {
   public static String order(String words) {
        String[] arr = words.split(" ");
        StringBuilder result = new StringBuilder("");
        for (int i = 0; i < 10; i++) {
            for (String s : arr) {
                if (s.contains(String.valueOf(i))) {
                    result.append(s + " ");
                }
            }
        }
        return result.toString().trim();
    }
}
```

* 이중 for문을 쓰긴 하지만, 간단한 구현이기 때문에 직관적으로 이해가 간다.

  이 방법이 더 좋은거 같기도 하다..

#### 모르는거 공부

* 정규표현식 2

  * 문자열 포함여부 확인하기위해 보통 String의 contains()메서드를 사용한다.
  * matches()메서드로 확인하는 방법
    * matches(".\*<원하는문자열>.*");
    * ex) 문자열에 숫자가 포함되어있는지 확인하려면 --> matches(".\*[0-9].*")

* Stream의 reduce

  * Stream의 데이터를 변환하지 않고, 더하거나 빼는 등의 연산을 수행하여 하나의 값을 ㅗ만들 수 있다.

    ```java
    Stream<Integer> numbers = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    Optional<Integer> sum = numbers.reduce((x, y) -> x + y);
    ```

    * stream의 1, 2를 더한 값 3을 다시 x에 할당하고 그 다음 값 3을 y에 할당한다.

    * 1 + 2 --> (1+2) + 3 --> ((1+2) + 3)  + 4 --> (((1+2)+3)+4) +5 --> 이런식..
    * 위 문제에서 `((a, b) -> a + " " + b)`는 숫자 순서대로 정렬된 stream을 차례로 돌면서 문자열을 붙여나가는것이다.
