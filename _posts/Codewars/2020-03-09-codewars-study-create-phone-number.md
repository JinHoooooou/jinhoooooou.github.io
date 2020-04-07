---
title: "Codewars 문제풀기 (03/09)"
excerpt: "Create Phone Number?"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-09
---



## [Create Phone Number](https://www.codewars.com/kata/525f50e3b73515a6db000b83/train/java)

* 배열 길이가 10인 int형 배열을 인자로 받는다

* 배열 인자를 차례대로 이용하여 (123) 456-7890의 포맷으로 출력한다.

* 길이는 항상 10이라고 가정하고 자연수라고 가정한다.

  


#### 1. Test를 만들었다

* 배열 {1,2,3,4,5,6,7,8,9,0} 인 배열

  그냥 `String.format()`메소드를 쓰면 되지않나..

  ```java
  public class Kata {
    public static String createPhoneNumber(int[] numbers) {
      return String.format("(%d%d%d) %d%d%d-%d%d%d%d",numbers[0],numbers[1],numbers[2],numbers[3],numbers[4],numbers[5],numbers[6],numbers[7],numbers[8],numbers[9]);
    }
  }
  ```

  제출


####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Kata {
  public static String createPhoneNumber(int[] numbers) {
    return String.format("(%d%d%d) %d%d%d-%d%d%d%d",numbers[0],numbers[1],numbers[2],numbers[3],numbers[4],numbers[5],numbers[6],numbers[7],numbers[8],numbers[9]);
  }
}
```

* 같다.

두번째로 많이 받은 코드

```java
public class Kata {
  public static String createPhoneNumber(int[] numbers) {
    return String.format("(%d%d%d) %d%d%d-%d%d%d%d", java.util.stream.IntStream.of(numbers).boxed().toArray());
  }
}
```

* 스트림으로 표현 할 수 있겠다 라고 생각은 했는데 아직 람다까지밖에 공부를 안해서 어떤 메소드를 써서 구현할 수 있을까 생각은 못했다.

