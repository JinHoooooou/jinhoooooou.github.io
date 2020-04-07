---
title: "Codewars 문제풀기 (03/02)"
excerpt: "Highest And Lowest"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-02

---

## [Highest and Lowest](https://www.codewars.com/kata/554b4ac871d6813a03000035/train/java)

* 정수로 구성된 문자열을 인자로 받아서 '최대값 최소값' 포맷으로 출력

* highAndLow("1 2 3 4 5") --> "5 1"

* 입력 문자열은 1개 이상의 숫자로 구성

  


#### 1. Test를 만들었다

* 숫자가 하나일 때 

  입력한 숫자를 '최대값 최소값' 포맷으로 출력하면 되겠구나

  ```java
  public static String highAndLow(String numbers){
        return numbers + " " + numbers;
  }
  ```

* 숫자가 두개 이상일 때

  문자열 중 최대값과 최소값을 찾는 로직을 구현하면 되겠구나

  ```java
  public static String highAndLow(String numbers){
        String[] numberList = numbers.split(" ");
        int highestNumber = Integer.parseInt(numberList[0]);
        int lowestNumber = Integer.parseInt(numberList[0]);
  
        for(String target : numberList) {
            int parseToInt = Integer.parseInt(target);
            if(parseToInt > highestNumber) {
                highestNumber = parseToInt
            }
            if(parseToInt < lowestNumber) {
                lowestNumber = parseToInt
            }
        }
      return highestNumber + " " + lowestNumber;
  }
  ```
  
  Success. 

#### 2. 리팩토링 시작

if문으로 비교하는거보다 Math.max, min을 사용하는게 더 깔끔할거 같아서 사용했다.

```java
public static String highAndLow(String numbers) {
    String[] numberList = numbers.split(" ");
    int highestNumber = Integer.parseInt(numberList[0]);
    int lowestNumber = Integer.parseInt(numberList[0]);
    
    for(String target : numberList) {
        int parseToInt = Integer.parseInt(target);
        highestNumber = Math.max(higestNumber, parseToInt);
        lowestNumber = Math.min(lowestNumber, parseToInt);
    }
    return highestNumber + " " + lowestNumber;
}
```

max, min 메소드를 사용하고 보니까, highest, lowest도 그냥 max, min으로 고쳐도 될거같다 라고 생각했는데 그냥 제출했다.

#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public static String highAndLow(String numbers){
    int min = Arrays.stream(numbers.split(" "")).mapToInt(Integer::parseInt).min().getAsInt();
    int max = Arrays.stream(numbers.split(" ")).mapToInt(Integer::parseInt).max().getAsInt();
    return String.format("%d %d", max,min);
}
```

* 스트림과 람다식을 이용하면 매우 간단하게 풀 수 있구나 라고 느꼈다. 근데 아직 스트림, 람다에 대해 잘 모르므로 공부할필요가 있다고 느꼈다.



내가 푼 방법과 가장 비슷한 코드

```java
public class Kata {
  public static String HighAndLow(String numbers) {
     int max = Integer.MIN_VALUE;
     int min = Integer.MAX_VALUE;
     String nums[] = numbers.split(" ");

     for(String s: nums) {
       int num = Integer.parseInt(s);

       max = Math.max(max, num);
       min = Math.min(min, num);
     }

     return "" + max + " " + min;
  }
}
```

* max, min으로 이름짓는게 더 보기 편하다고 느꼈다.
* Integer.MIN_VALUE, MAX_VALUE가 있구나 이걸로 표현하는게 더 낫겠다.


#### 4. 궁금한거 공부

* 스트림과 람다

  'Effective Java' 책에 한 챕터로 구분될 만큼 양이 많기 때문에 따로 공부해야겠다.

  간단하게 알아보니

  스트림 생성 - 가공 - 처리 개념이라고 한다.

  Arrays.stream(numbers.split(" "))으로 생성

  mapToInt(Integer::parseInt).min()으로 가공

  getAsInt()로 처리

  개념으로 보면 될거같은데 관련 API가 매우 많으니 한번 깊게 알아볼 필요가 있을것 같다.