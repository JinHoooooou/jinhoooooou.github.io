---
title: "Codewars 문제풀기 (05/04)"
excerpt: "Sort the odd (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-04
---



# [Sort the odd](https://www.codewars.com/kata/578aa45ee9fd15ff4600090d/train/java)

* int[] array를 인자로 받는다.

* array의 짝수는 그 위치 그대로 두고 홀수만 정렬해서 리턴한다.

* array가 empty array거나 null이라면 array empty를 리턴한다.

  ```
  sortArray({5, 3, 2, 8, 1, 4}) 👉 {1, 3, 2, 8, 5, 4}
  sortArray({5, 3, 1, 8, 0}) 👉 {1, 3, 5, 8, 0}
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력 배열이 빈 배열일 때 빈 배열을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return empty array when input is empty array")
    public void test1() {
      // Given: Set empty array
      int[] given = new int[]{};
    
      // Then: Should return empty array
      assertArrayEquals(given, Kata.sortArray(given));
    }
    ```

  * 실제 코드

      ```java
      public class Kata {
      
        public static int[] sortArray(int[] array) {
      
          if (array == null || array.length == 0) {
            return new int[]{};
          }
          return array;
        }
      }
      
      ```
      
      * null인 경우도 같이 구현했다.

* ### 테스트 2 - 입력 배열이 홀수로만 구성되어 있을 경우 전체 sort해서 리턴한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return ascending sorted array when input has only odd elements")
    public void test2() {
      // Given: Set array has only odd number
      int[] given = new int[]{7, 3, 5, 1, 9, 17, 13, 29, 45};
    
      // Then: Should return ascending sorted array
      assertArrayEquals(new int[]{1, 3, 5, 7, 9, 13, 17, 29, 45}, Kata.sortArray(given));
    }
    ```
    
  * 실제 코드

    ```java
  public class Kata {
    
      public static int[] sortArray(int[] array) {
    
        if (array == null || array.length == 0) {
          return new int[]{};
        }
        Arrays.sort(array);
        return array
      }
    }
    ```
    
    * 단순히 array를 sort해서 리턴했다.

* ### 테스트 3 - 짝수는 위치 그대로 두고 홀수만 정렬한다.

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return even number be their places")
    public void test3() {
      // Given: Set array has odd and even number
      int[] given = new int[]{5, 3, 2, 8, 1, 4};
    
      // Then: Should return ascending sorted array
      assertArrayEquals(new int[]{1, 3, 2, 8, 5, 4}, Kata.sortArray(given));
    }
    ```

  * 실제 코드

    ```java
    public class Kata {
    
      public static int[] sortArray(int[] array) {
    
        if (array == null || array.length == 0) {
          return new int[]{};
        }
        List<Integer> oddSortedList = new ArrayList<>();
        for (int i = 0; i < array.length; i++) {
          if (array[i] % 2 == 1) {
            oddSortedList.add(array[i]);
          }
        }
        oddSortedList = oddSortedList.stream().sorted().collect(Collectors.toList());
        int oddSortedListIndex = 0;
        for (int i = 0; i < array.length; i++) {
          if (array[i] % 2 == 1) {
            array[i] = oddSortedList.get(oddSortedListIndex);
            oddSortedListIndex++;
          }
        }
    
        return array;
      }
    }
    ```

    * List에 array의 홀수만 저장한 뒤 정렬했다. 그리고 array에서 element가 홀수라면 List에 저장된 값을 순서대로 저장하고 짝수라면 그냥 넘어간다.

---

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class KataBestPractice {

  public static int[] sortArray(int[] array) {

    int[] oddSortedArray = IntStream.of(array).filter(x -> x % 2 == 1).toArray();
    for (int i = 0, oddSortedArrayIndex = 0; i < array.length; i++) {
      if (array[i] % 2 == 1) {
        array[i] = oddSortedArray[oddSortedArrayIndex++];
      }
    }

    return array;
  }
}

```

* 방법은 같은데 훨씬 더 짧게 구현했다.
* 굳이 List에 저장하지 않고, 배열에 저장해도 되는데 나는 왜 List에 저장했을까.. 생각해보면 리팩토링할때 좀 더 깊이 고민하지 않는다. 문제 통과하는데에 만족을 해버려서 그런것 같다. 이 부분이 굳이 필요한가? 한번 더 생각해보고 리팩토링하고 제출하도록 해야할것같다 ㅜ
