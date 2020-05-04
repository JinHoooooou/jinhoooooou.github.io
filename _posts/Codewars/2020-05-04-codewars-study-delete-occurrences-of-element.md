---
title: "Codewars 문제풀기 (05/04)"
excerpt: "Delete occurrences of an element if it occurs more than n times (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-04
---



# [Delete occurrences of an element if it occurs more than n times](https://www.codewars.com/kata/554ca54ffa7d91b236000023/train/java)

* int[] elements, int maxOccurrences를 인자로 받는다.

* elements 배열의 elemtent들 중 maxOccurrences보다 더 많은 element들을 maxOccurrences만큼만 남기고 삭제한다.

  ```
  deleteNth(new int[]{20,37,20,21},1) 👉 {20,37,21}
  deleteNth(new int[]{1,1,1,3,3,3,7,2,2,2,2},2) 👉 {1,1,3,3,7,2,2}
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 빈 배열일 때 maxOccurrences에 상관없이 빈 배열을 리턴한다.

  * 테스트 코드

    ```java
    @Test
    public void testShouldEmptyArrayWhenInputArrayIsEmptyRegardlessOfMaxOccurrences() {
      // Given: Set empty array and maxOccurrences
      int[] givenArray = new int[]{};
      int givenMaxOccurrences = 5;
    
      // Then: Should Return empty array regardless maxOccurrences
      assertArrayEquals(givenArray, EnoughIsEnough.deleteNth(givenArray, givenMaxOccurrences));
    }
    ```
    
    * When없이 Then으로 처리해도 괜찮다고 생각해서 없앴다.
    
  * 실제 코드

      ```java
      public class EnoughIsEnough {
      
        public static int[] deleteNth(int[] elements, int maxOccurrences) {
          return elements.length==0 ? new int[]{} : null;
        }
      }
      ```



* ### 테스트 2 - {20,37,20,21}, 1가 인자일때 {20,37,20}을 리턴한다

  * 테스트 코드

    ```java
    @Test
    @DisplayName("test should return {20, 37, 21} when input is {20, 37, 20, 21} and 1")
    public void test1() {
      // Given: Set array {20, 37, 20, 21} and maxOccurrences 1
      int[] givenArray = new int[]{20, 37, 20, 21};
      int givenMaxOccurrences = 1;
    
      // Then: Should Return {20, 37, 21}
      assertArrayEquals(new int[]{20, 37, 21},
          EnoughIsEnough.deleteNth(givenArray, givenMaxOccurrences));
    }
    ```

  * 실제 코드

    ```java
    public class EnoughIsEnough {
    
      public static int[] deleteNth(int[] elements, int maxOccurrences) {
        if (elements.length == 0) {
          return new int[]{};
        }
        Map<Integer, Integer> valueCount = new HashMap<>();
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < elements.length; i++) {
          valueCount.put(elements[i], 0);
        }
    
        for (int i = 0; i < elements.length; i++) {
          if (valueCount.get(elements[i]) < maxOccurrences) {
            result.add(elements[i]);
            valueCount.put(elements[i], valueCount.get(elements[i]) + 1);
          }
        }
        return result.stream().mapToInt(i -> i).toArray();
      }
    }
    ```

    * value와 value count를 Map을 이용해서 key value로 저장했다. 그래서 List에 element들을 maxOccurrences이하 만큼만 저장한 뒤 array로 변환해서 리턴했다.
    * for문을 잘 보면 같은 for문이기 때문에 하나로 합칠 수 있지 않을까? 생각을 했는데 그러면 for문 돌 때마다 0으로 초기화 되어서 그냥 분리해서 구현했다.

---

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.*;

public class EnoughIsEnough {

  public static int[] deleteNth(int[] elements, int max) {
  
    if (max < 1) return new int[0];
    
    final HashMap<Integer,Integer> map = new HashMap<>();
    final List<Integer> list = new ArrayList<>();
    
    for (final Integer i : elements) {
      final Integer v = map.put(i, map.getOrDefault(i, 0) + 1);
      if (v == null || v < max) list.add(i);
    }
    
    return list.stream().mapToInt(i->i).toArray();
  }

}
```

* 구현하는 방법 자체는 같은데 더 좋은방법을 썼다.
* `Map.put()`의 리턴값이 이전 value값인것도 몰랐고, `map.getOrDefault()`으로 Key가 없으면 default값을 리턴하도록 할 수 있는 메서드가 있는것도 몰랐다. 애초에 map이 key value로 구성되어있다. 라고만 알고 있고 지원하는 메서드는 잘 몰랐다.
* 전에 문제 몇개를 통해서 알게 된것이지만 List 👉 Array로 변환하는 방법이 은근 유용하다고 문제 풀때마다 느낀다.

