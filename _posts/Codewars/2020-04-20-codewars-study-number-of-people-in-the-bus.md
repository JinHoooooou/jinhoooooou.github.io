---
title: "Codewars 문제풀기 (04/20)"
excerpt: "Number of People in the Bus(7kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-20
---



## [Number of People in the Bus](https://www.codewars.com/kata/5648b12ce68d9daa6b000099/train/java)

* `ArrayList<int[]>`를 인자로 받는다.

* int[]의 크기는 2 이며, {버스 탄 사람, 버스에서 내린 사람}이다.

* 최종 버스에 남은 사람을 리턴한다.

*     ArrayList<int[]> list = new ArrayList<int[]>();
      list.add(new int[] {10,0}); 👉 counterPassengers(list) 👉 10
      list.add(new int[] {3,5}); 👉 counterPassengers(list) 👉 8
      list.add(new int[] {2,5}); 👉 counterPassengers(list) 👉 5

#### 1. Test와 리팩토링

* 테스트 1 - list가 {0, 0}이면 0을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn0WhenBusIsEmpty() {
      // Given: Set get in, and get out people 0
      ArrayList<int[]> inAndOutPeopleList = new ArrayList<>();
      inAndOutPeopleList.add(new int[]{0, 0});
      // When: Call countPassengers method
      int actual = Metro.countPassengers(inAndOutPeopleList);
      // Then: Should return 0
      assertEquals(0, actual);
    }
    ```
    
  * 실제 코드
  
    ```java
    import java.util.ArrayList;
    
    public class Metro {
    
      public static int countPassengers(ArrayList<int[]> stops) {
        return 0;
      }
    }
  ```
  
  * 리팩토링 1
  
    ```java
    import java.util.ArrayList;
    
    public class Metro {
    
      public static int countPassengers(ArrayList<int[]> stops) {
        int peopleInBus = 0;
        for (int i = 0; i < stops.size(); i++) {
          peopleInBus = peopleInBus + stops.get(i)[0];
          peopleInBus = peopleInBus - stops.get(i)[1];
        }
        return peopleInBus;
      }
    }
  ```
  
  * for문을 돌면서 각 int[] element의 첫번째 element는 탄 사람 수, 두번째 element는 내린 사람 수 이므로 계속 더해줬다.
  
  * 리팩토링 2 - 식을 한번으로 줄이고, 향상된 for문 사용
  
    ```java
    import java.util.ArrayList;
    
    public class Metro {
    
      public static int countPassengers(ArrayList<int[]> stops) {
        int peopleInBus = 0;
        for (int[] stop : stops) {
          peopleInBus += stop[0] - stop[1];
        }
        return peopleInBus;
      }
    }
    ```

* 테스트 2 - 첫번째 stop에서 10명 타고 0명 내리면 10을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturn10WhenPeopleGetInBus10AndOff0() {
      // Given: Set first stop : get in, get out people 0
      // Given: Set second stop : get in 10, get out 0
      ArrayList<int[]> inAndOutPeopleList = new ArrayList<>();
      inAndOutPeopleList.add(new int[]{0, 0});
      inAndOutPeopleList.add(new int[]{10, 0});
    
      // When: Call countPassengers method
      int actual = Metro.countPassengers(inAndOutPeopleList);
      
      // Then: Should return 10
      assertEquals(10, actual);
    }
    ```

  * 실제 코드 그대로

* Stream으로도 풀 수 있을 것 같은데.. ArrayList<int[]>인데 int[]로 어떻게 해결할지 잘 몰라서 그냥 이대로 제출했다. (약 10분)

#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.ArrayList;

class Metro {

  public static int countPassengers(ArrayList<int[]> stops) {
    return stops.stream()
                .mapToInt(x -> x[0] - x[1])
                .sum();
  }
}
```

* x가 int[]형인데 x[0] - x[1]을 x에 저장할 수 있나..? 스트림을 잘 모르니 구조도 이해가 잘 안된다.. 공부해야하는데 흑
