---
title: "Codewars 문제풀기 (03/08)"
excerpt: "Descending Order"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-08
---



## [Descending Order](https://www.codewars.com/kata/5467e4d82edf8bbf40000155/train/java)

* 자연수를 인자로 받는다.
* digit descending oreder로 출력한다.
* sortDesc(21455) --> 54421
* sortDesc(145263) --> 654321


#### 1. Test를 만들었다

* 인자가 1 digit 일 때

  그냥 입력 인자 출력

  ```java
  public static int sortDesc(final int num) {
      return num;
  }
  ```

* 인자가 2 digits 이상 일 때

  List<>나, Collections를 이용해서 내림차순 정렬을 할 수 있으니까 List에 digit을 넣어서 정렬하고 String으로 하나씩 붙여서 int로 변환해서 출력하면 되겠다.

  ```java
  import java.util.ArrayList;
  import java.util.Comparator;
  import java.util.List;
  
  public class DescendingOrder {
  
    public static int sortDesc(final int num) {
      String parseToString = String.valueOf(num);
      List<Integer> digitList = new ArrayList<>();
      for (int i = 0; i < parseToString.length(); i++) { 
          digitList.add(Integer.parseInt(String.valueOf(parseToString.charAt(i))));
      }
          StringBuilder result = new StringBuilder();
      for (Integer integer : digitList) {
        result.append(integer);
      }
      return Integer.parseInt(result.toString());
    }
  ```

  List에 digit들을 저장하는 방법을 생각 많이했다.  처음에는 toCharArray메소드로 char 배열을 만들어 List에 저장하려 했는데 안됐다. 그래서 String.to % 10 연산으로 List에 추가할지, String으로 변환해서 하나씩 저장할지 고민하다가 후자를 선택했다.

  StringBuilder에 append하는 방법말고 int형에 10의 거듭제곱을 더하는 방법을 처음에 생각했는데, 굳이 그럴필요 없다고 생각하여 그냥 String에 append하는 방법을 선택했다.

#### 2. 리팩토링 시작

더 짧은 코드로 해결 할 수 있을거 같은데.. 라는 생각만하고 자바에 대한 지식이 부족해서 그러지 못했다.  그래서 메소드로 추출하여 줄이려고 노력했다.

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

public class DescendingOrder {

  public static int sortDesc(final int num) {
    String parseToString = String.valueOf(num);
    String result = "";
    List<Integer> digitList = makeDigitList(parseToString);
    digitList.sort(Comparator.reverseOrder());

    result = makeDescendingOrderString(digitList);

    return Integer.parseInt(result);
  }

  private static String makeDescendingOrderString(List<Integer> digitList) {
    StringBuilder result = new StringBuilder();
    for (Integer integer : digitList) {
      result.append(integer);
    }
    return result.toString();
  }

  private static List<Integer> makeDigitList(String parseToString) {
    List<Integer> digitList = new ArrayList<>();
    for (int i = 0; i < parseToString.length(); i++) {
      digitList.add(Integer.parseInt(String.valueOf(parseToString.charAt(i))));
    }
    return digitList;
  }
}
```

제출

####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Comparator;
import java.util.stream.Collectors;

public class DescendingOrder {
    public static int sortDesc(final int num) {
        return Integer.parseInt(String.valueOf(num)
                                      .chars()
                                      .mapToObj(i -> String.valueOf(Character.getNumericValue(i)))
                                      .sorted(Comparator.reverseOrder())
                                      .collect(Collectors.joining()));
    }
}
```

* String.chars()의 리턴 타입이 IntStream인데 Stream과 Lamda를 이용해서 해결했다. getNumericValue, Collectors.joining은 처음봤다.

Best Practice 두번째로 많이 받은 코드

```java
import java.util.Arrays;
import java.util.Collections;

public class DescendingOrder {
    public static int sortDesc(final int num) {
        String[] array = String.valueOf(num).split("");
        Arrays.sort(array, Collections.reverseOrder());
        return Integer.valueOf(String.join("", array));
    }
}
```

* split("")으로 digit 추출하는 방법도 있다는걸 알게되었고 List나 Collection말고 Arrays에도 sort 메소드가 있는걸 알게 되었다. 그리고 join메소드로 배열의 element들을 연결 할 수도 있다는걸 알았다.


#### 4. 궁금한거 공부

* IntStream -->  일단 스트림이니까 람다가 나올거라고 예상이되니까 스트림도 알아야하고 람다도 알아야한다. 그리고 Stream이랑 IntStream차이는 무엇?
* Collectors --> collectors의 joining, Collector와 Collectors차이는?
* List와 Collection --> Collection과 Collections 차이는?
* String.join --> String 클래스에 대해
* Character.getNumericValue --> Character 클래스에 대해 정리
