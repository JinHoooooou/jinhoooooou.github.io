---
title: "Codewars 문제풀기 (05/16)"
excerpt: "Maximum subarray sum (5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-16
---



# [Maximum subarray sum](https://www.codewars.com/kata/54521e9ec8e60bc4de000d6c/train/java)

* int array를 입력으로 받는다.

* 연속된 sub array중 합이 가장 큰 sub array의 합을 리턴한다.

  ``` 
  Max.sequence([-2, 1, -3, 4, -1, 2, 1, -5, 4]) 👉 6 ({4, -1, 2, 1})
  ```

  



## 1. Test와 리팩토링

* ### 테스트 1 - 입력 배열이 empty 배열일 때 0을 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/c49c1e174808f3d165484f9c53c9a3118d74ed81)

    ```java
    @Test
    @DisplayName("test should return 0 when input array empty")
    public void test1() {
      // Given: Set empty array
      int[] given = {};
    
      // When: Call sequence method
      int actual = Max.sequence(given);
    
      // Then: Should return 0
      assertEquals(0, actual);
    }
    ```

    

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/c2a7f80bf102f2850f41c65b11675bec95c35189)

    ```java
    public class Max {
    
      public static int sequence(int[] arr) {
        return arr.length == 0 ? 0 : 1;
      }
    }.
    ```

* ### 테스트 2 - 입력 배열의 원소가 모두 양수일 때 배열의 합을 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/30d5e126bc113682d3c5cf872bd5d8cbda20cf0c)

    ```java
    @Test
    @DisplayName("test should return sum all element when input element all positive")
    public void test2() {
      // Given: Set array element all positive
      int[] given = {1,2,3,4,5};
    
      // When: Call sequence method
      int actual = Max.sequence(given);
    
      // Then: Should return 15
      assertEquals(15, actual);
    }
  ```
  
    
  
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/501b3fb150fec9405af54d8a6d45823e3ae95eae)
  
    ```java
    import java.util.stream.IntStream;
    
    public class Max {
    
      public static int sequence(int[] arr) {
        if (arr.length == 0) {
          return 0;
        }
        return IntStream.of(arr).sum();
      }
    }
    ```
  
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/add6e0537a01f634bba5c543ef04404f914570e9)
  
    ```java
    @Test
    @DisplayName("test should return 0 when input array empty") 
    public void test1() {
      // Given: Set empty array
      int[] given = {};
    
      // Then: Should return 0
      assertEquals(0, Max.sequence(given));
    }
    
    @Test
    @DisplayName("test should return sum all element when input element all positive")
    public void test2() {
      // Given: Set array element all positive
      int[] given = {1,2,3,4,5};
    
      // Then: Should return 15
      assertEquals(15, Max.sequence(given));
    }
    ```
  
    * When 삭제하고 Then에 이어붙여줌

* ### 테스트 3 - 입력 배열의 원소가 모두 음수일 때 0을 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/25aa4333f236ccc040245d8a95bdc59e5b5c72db)

    ```java
    @Test
    @DisplayName("test should return 0 when input element all negative")
    public void test3() {
      // Given: Set array element all negative
      int[] given = {-11, -2, -43, -124, -65};
    
      // Then: Should return 0
      assertEquals(0, Max.sequence(given));
    }
    ```

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/a3a36b89ef1503d5182fa5fcedc2cad9d5368931)

    ```java
    public class Max {
      public static int sequence(int[] arr) {
        if (arr.length == 0) {
          return 0;
        }
        int sum = IntStream.of(arr).sum();
        return Math.max(sum, 0);
      }
    }
    ```

* ### 테스트 4 - 입력 배열이 [-2, 1, -3, 4, -1, 2, 1, -5, 4] 일 때 6을 리턴한다

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/9bdbe8848189573430b8dbaeaf087a7b7e42b023)

    ```java
    @Test
    @DisplayName("test should return 6 when input [-2, 1, -3, 4, -1, 2, 1, -5, 4]")
    public void test4() {
      // Given: Set array
      int[] given = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    
      // Then: Should return 6
      assertEquals(6, Max.sequence(given));
    }
    ```

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/bbf477a99be15b4bcaf6d2af7cf3e5da8842e021)

    ```java
    public class Max {
    
      public static int sequence(int[] arr) {
        int max = 0;
        for (int start = 0; start < arr.length; start++) {
          for (int end = start + 1; end < arr.length; end++) {
            int sum = 0;
            for (int i = start; i <= end; i++) {
              sum += arr[i];
            }
            max = Math.max(max, sum);
          }
        }
        return max;
      }
    }
    ```

    * 3중 for문이라서 엄청 맘에 들지 않았지만 다른 방법이 생각이 안나서 이렇게 해결했다. element 1개 sub array에 대한 최대값, 2개, 3개 ... 전부 구해서 합이 가장 큰 값을 리턴했다.



## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Max {

    public static int sequence(int[] arr) {
        int max_ending_here = 0, max_so_far = 0;
        for (int v : arr) {
            max_ending_here = Math.max(0, max_ending_here + v);
            max_so_far = Math.max(max_so_far, max_ending_here);
        }
        return max_so_far;
    }
}
```

* for문 하나로 해결했다. 처음에는 봐도 무슨 소리인가 싶었는데 직접 코드를 실행해보니 이해가 됐다.
* 시작지점을 index 0부터 하나씩 더해가는데 더한 값이 0보다 작으면 index 위치를 한 칸 이동한다. 문제를 풀 때는 이렇게 하면 안될거라고 생각했다. 왜냐하면 "0보다 작더라도 뒤의 값이 큰 값이 나오면 상관없지 않나?" 라는 생각에 그렇게 했다. 답을 보고 다시 생각해보니까 0보다 작아지면 그 index들을 포함하지 않는게 더 큰 값을 리턴한다는 것을 알 수 있었다.
* 내가 푼 방식에 큰 자괴감을 느껴서 지인들에게 문제를 공유했더니 Dynamic Programming 문제라고한다. 이 부분 공부가 필요한것같다. 옛날에 Longest Subsequence 문제를 배웠었는데 지금 생각해보니까 그 문제랑 비슷한것같다. 배워놓고 써먹지를 못하네 ㅂㄷㅂㄷ 야발!
