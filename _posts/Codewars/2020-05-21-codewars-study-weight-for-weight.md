---
title: "Codewars 문제풀기 (05/21)"
excerpt: "Weight for weight (5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-21
---



# [Weight for weight](https://www.codewars.com/kata/55c6126177c9441a570000cc/train/java)

* String을 입력으로 받는다.

* String은 Valid integer로 이루어져 있고 space로 구분되어 있다.

* integer들을 정렬하여 String으로 리턴한다.

* 정렬 규칙은 integer의 각 digit를 더하여 오름차순으로 정렬한다.

* digit들을 더한 값이 같다면 String 오름차순으로 정렬한다.

  ``` 
  WeightSort.orderWeight("103 123 4444 99 2000")
  👉 103 = 1+0+3=4, 123 = 1+2+3=6, 4444=12, 99=18, 2000=2
  👉 "2000 103 123 4444 99"
  WeightSort.orderWeight("2000 10003 1234000 44444444 9999 11 11 22 123")
  👉 2000 = 2, 10003 = 4, 1234000 = 10, 444444444 = 36, 9999 = 36, 11 = 2, 11 = 2, 22 = 4, 123 = 6
  👉 11과 2000은 point가 같지만 String 오름차순이면 11이 2000보다 앞에 와야한다.
  👉 "11 11 2000 10003 22 123 1234000 44444444 9999"
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 "103 123 4444 99 2000"일 때 "2000 103 123 4444 99"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/f9fb920cfe98546b999ad181210dba3b94134e0e)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/b38b18698d59dafa9b51c89896c3f4120585c446)
  
    ```java
  import java.util.Map;
    import java.util.TreeMap;
    
    public class WeightSort {
    
      public static String orderWeight(String string) {
        String[] weightList = string.split(" ");
        Map<Integer, String> map = new TreeMap<>();
    
        for (String weight : weightList) {
          int key = 0;
          for (Character c : weight.toCharArray()) {
            key += Character.getNumericValue(c);
          }
          if (!map.containsKey(key)) {
            map.put(key, weight);
          } else {
            map.put(key, map.get(key) + " " + weight);
          }
        }
        String result = "";
        for (Integer key : map.keySet()) {
          result += map.get(key) + " ";
        }
        return result.trim();
      }
    }
    ```
    
    * String의 weight들의 digit들을 더해서 digit들을 더한값 - weight을 Map을 이용해서 Key, Value로 저장했다. Key들을 오름차순으로 저장하기 위해서 HashMap이 아닌 TreeMap을 사용했다. TreeMap을 사용하면 `map.keySet()`으로 key들을 가져올 때 key값이 정렬된 상태로 가져 온다.
    
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/756975157d4ff7a8e1a09db1fae4e5429df8aec9)
  
    ```java
    public class WeightSort {
    
      public static String orderWeight(String string) {
        Map<Integer, String> map = getWeightMap(string);
        return getResultString(map);
      }
    
      private static String getResultString(Map<Integer, String> map) {
        String resultString = "";
        for (Integer key : map.keySet()) {
          resultString += map.get(key) + " ";
        }
        return resultString.trim();
      }
    
      private static Map<Integer, String> getWeightMap(String string) {
        Map<Integer, String> map = new TreeMap<>();
        for (String weight : string.split(" ")) {
          int key = getWeightPoint(weight);
          map.put(key, weight);
        }
        return map;
      }
    
      private static int getWeightPoint(String weight) {
        int weightPoint = 0;
        for (Character c : weight.toCharArray()) {
          weightPoint += Character.getNumericValue(c);
        }
        return weightPoint;
      }
    }
    ```
  
    * TreeMap을 만드는 부분, 각 weight의 digit들을 더하는 부분, resultString을 만드는 부분을 method로 추출했다.
  
* ### 테스트 2 - 입력이 "2000 10003 1234000 44444444 9999 11 11 22 123"일 때 "11 11 2000 10003 22 123 1234000 44444444 9999"를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/2d9793054339a902ae742f3ac305598323ee8b39)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/6d1e6215dd1498941eaa0fe0ab5ddfa081d9a76d)

    ```java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Map;
    import java.util.TreeMap;
    
    public class WeightSort {
    
      public static String orderWeight(String string) {
        Map<Integer, ArrayList<String>> map = getWeightMap(string);
        return getResultString(map);
      }
    
      private static String getResultString(Map<Integer, ArrayList<String>> map) {
        String resultString = "";
        for (Integer key : map.keySet()) {
          ArrayList<String> weightList = map.get(key);
          Collections.sort(weightList);
          for (String weight : weightList) {
            resultString += weight + " ";
          }
        }
        return resultString.trim();
      }
    
      private static Map<Integer, ArrayList<String>> getWeightMap(String string) {
        Map<Integer, ArrayList<String>> map = new TreeMap<>();
        for (String weight : string.split(" ")) {
          ArrayList<String> valueList = new ArrayList<>();
          int key = getWeightPoint(weight);
          if (map.containsKey(key)) {
            valueList = map.get(key);
          }
          valueList.add(weight);
          map.put(key, valueList);
        }
        return map;
      }
      ...
    }
    ```

    * digit들을 더한 weightPoint들이 같은 경우도 있기 때문에 map의 value를 List로 받아서 List에 weigth들을 저장했다. 그리고 resultStirng을 만들기 전에 List를 정렬 한 뒤 만들어 주었다.

  * [리팩토링1](https://github.com/JinHoooooou/codeWarsChallenge/commit/f3ce3f7ec438cfca3753fdf4aa8e31e43026200d)

    ```java
    public class WeightSort {
    
    public static String orderWeight(String string) {
        Map<Integer, ArrayList<String>> map = getWeightMap(string);
        return String.join(" ", map.values());
      }
      ...
    }
    ```
    
    * map에 있는 element들을 `map.get()`을 통해 가져와서 String에 붙일 필요 없이 `String.join()`을 사용하여 만들 수 있다. 그래서 `getResultStirng()` 메서드를 삭제했다
    
  * [리팩토링2](https://github.com/JinHoooooou/codeWarsChallenge/commit/08811a4964bb9e82f0c0ff3d317910ac2f7d727e)

    ```java
    public class WeightSort {
      ...
        
      private static Map<Integer, ArrayList<String>> getWeightMap(String string) {
        Map<Integer, ArrayList<String>> map = new TreeMap<>();
        for (String weight : string.split(" ")) {
          ArrayList<String> valueList = new ArrayList<>();
          int key = weight.chars().map(Character::getNumericValue).sum();
          if (map.containsKey(key)) {
            valueList = map.get(key);
          }
          valueList.add(weight);
          map.put(key, valueList);
        }
        return map;
      }
    ```

    * 각 digit들을 더하는 부분을 따로 메서드를 만들지 않고 스트림으로 구현했다. 그래서 `getWeightPoint()`메서드를 삭제했다.

  * [리팩토링3](https://github.com/JinHoooooou/codeWarsChallenge/commit/f3fa64ed7963c7a8f993fc34364d9dd3944668ae)

    ```java
    import java.util.Arrays;
    import java.util.Map;
    import java.util.TreeMap;
    
    public class WeightSort {
    
      public static String orderWeight(String string) {
        Map<Integer, String> map = getSortedWeightMap(string);
        return String.join(" ", map.values());
      }
    
      private static Map<Integer, String> getSortedWeightMap(String string) {
        Map<Integer, String> map = new TreeMap<>();
        String[] weightList = string.split(" ");
        Arrays.sort(weightList);
        for (String weight : weightList) {
          int key = weight.chars().map(Character::getNumericValue).sum();
          if (map.containsKey(key)) {
            weight = map.get(key) + " " + weight;
          }
          map.put(key, weight);
        }
        return map;
      }
    }
    ```

    * List들을 만들어 각 List마다 정렬을 하는것보다 처음 input String을 `String.split()`메서드로 배열로 만든 후에 정렬을 하는 것이 더 좋아보인다. 그리고 Map의 Value로 List를 사용하는것 보다 String을 사용하는것이 훨씬 보기 편해서 String을 사용했다.


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
import java.util.Comparator;
import java.util.Arrays;

public class WeightSort {
  public static String orderWeight(String string) {
    String[] split = string.split(" ");
    Arrays.sort(split, new Comparator<String>() {
      public int compare(String a, String b) {
        int aWeight = a.chars().map(c -> Character.getNumericValue(c)).sum();
        int bWeight = b.chars().map(c -> Character.getNumericValue(c)).sum();
        return aWeight - bWeight != 0 ? aWeight - bWeight : a.compareTo(b);
      }
    });
    return String.join(" ", split);
  }
}
```

* `Array.sort`에서 Comparator을 사용하여 정렬 기준을 아에 만들었다. 이렇게 하는것이 훨씬 간단해 보여서 좋다..
* 당연하게 Map으로 푸는거라 생각했는데 map을 이용하여 푼 답들이 별로 없었다 ㅜ

