---
title: "Codewars 문제풀기 (04/11)"
excerpt: "Equal Sides Of An Array(6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-04-10
---



## [Equal Sides Of An Array](https://www.codewars.com/kata/5679aa472b8f57fb8c000047/train/java)

* int배열을 인수로 받는다.
* 배열의 index를 기준으로 왼쪽 배열의 합과 오른쪽 배열의 합이 같으면 index를 리턴한다.
* 모든 index를 기준으로 왼쪽 배열과 오른쪽 배열의 합이 같은 index가 존재하지않으면 -1을 리턴한다.
* findEvenIndex({1,2,3,4,3,2,1}) --> index 3(4)를 기준으로 왼쪽 배열과 오른쪽 배열합이 같다 --> 3
* findEvenIndex({1,100,50,-51,1,1}) --> index 1(100)을 기준으로 왼쪽 배열과 오른쪽 배열합이 같다 --> 1
* findEvenIndex({20,10,-80,10,10,15,35}) --> index 0(20)을 기준으로 왼쪽 배열(0)과 오른쪽 배열 합이 0으로 같다 --> 0
* findEvenIndex({1,2,3,4,5,6}) --> 어떤 index를 기준으로 왼쪽 배열과 오른쪽 배열 합이 같지 않다 --> -1

#### 1. Test와 리팩토링

* 입력 배열의 어떠한 index 기준으로도 양 옆 배열 합이 같지 않다면 -1을 리턴해야한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldReturnMinus1WhenThereIsNoIndex() {
        // Given: Set int array no index of left number that equal sums of right numbers
        int[] given = {1, 2, 3, 4, 5, 6};
    
        // When: Call findEvenIndex method
        int actual = Kata.findEvenIndex(given);
    
        // Then: Should return -1
        assertEquals(-1, actual);
    }
    ```
    
  * 실제 코드

    ```java
    public class Kata {
    
      public static int findEvenIndex(int[] arr) {
        int leftSum = 0;
        int rightSum = 0;
    
        for (int targetIndex = 0; targetIndex < arr.length; targetIndex++) {
          leftSum = addLeftSum(arr, targetIndex);
          rightSum = addRightSum(arr, targetIndex);
          if (leftSum == rightSum) {
            return targetIndex;
          }
        }
        return -1;
      }
    
      private static int addRightSum(int[] arr, int targetIndex) {
        int sum = 0;
        for (int i = arr.length - 1; i > targetIndex; i--) {
          sum += arr[i];
        }
        return sum;
      }
    
      private static int addLeftSum(int[] arr, int targetIndex) {
        int sum = 0;
        for (int i = 0; i < targetIndex; i++) {
          sum += arr[i];
        }
        return sum;
      }
    ```
    
    * for문에서 index를 돌면서 index를 기준으로 leftSum과 rigthSum을 계산한뒤 두 값이 같은지 체크하고 같다면 index를 리턴하도록 했다. for문이 돌았는데도 없다면 -1를 리턴하도록 했다.
    

* 다른 테스트에도 적용해보니 통과해서 제출했다.

* 리팩토링

  * leftSum, rightSum자체를 메서드로 구현하면 지역변수가 필요없을것 같다

    ```java
    public class Kata {
    
      public static int findEvenIndex(int[] arr) {
        for (int targetIndex = 0; targetIndex < arr.length; targetIndex++) {
          if (leftSum(arr, targetIndex) == rightSum(arr, targetIndex)) {
            return targetIndex;
          }
        }
        return -1;
      }
    
      private static int rightSum(int[] arr, int targetIndex) {
        int sum = 0;
        for (int i = arr.length - 1; i > targetIndex; i--) {
          sum += arr[i];
        }
        return sum;
      }
    
      private static int leftSum(int[] arr, int targetIndex) {
        int sum = 0;
        for (int i = 0; i < targetIndex; i++) {
          sum += arr[i];
        }
        return sum;
      }
    ```
    
    * 답이 이중 for문을 사용해서 성능측면에서 조금 찝찝하긴 하다만, 스케일이 작기 때문에 그냥 제출했다.



#### 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Arrays;

public class Kata {
    public static int findEvenIndex(int[] arr) {
        int sumRight = Arrays.stream(arr).sum() - arr[0];
        int sumLeft = 0;
        for (int i = 1; i < arr.length; i++) {
            sumLeft += arr[i-1];
            sumRight -= arr[i];
            if (sumLeft == sumRight) return i;
        }
        return -1;
    }
}
```

* 제출하기전의 고민을 해결한 코드이다. 이중 for문을 사용하지 않아서 좋아보인다. 그런데 여기서 문제는 index 0이 나오지 않는다는점이다. for문전에 sumRight == sumLeft --> return 0 코드를 넣어줘야할것 같다.

