---
title: "Codewars 문제풀기 (05/26)"
excerpt: "Roman Numerals Encoder (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-26
---



# [Roman Numerals Encoder](https://www.codewars.com/kata/51b62bf6a9c58071c600001b/train/java)

* int를 입력으로 받는다.

* 입력받은 정수를 로마표기법으로 리턴한다.

  ``` 
  conversion.solution(1990) 👉 MCMXC
  conversion.solution(2008) 👉 MMVIII
  conversion.solution(1666) 👉 MDCLXVI
  ```

  


## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 1,2,3일때 I,II,III를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/0632814439363114fc77c81a07d1316de6771e53)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/13258757f4c652af0dc0d8fb5568caff8ca89b7d)
    * 반복문을 이용해서 "I"를 붙여주고 입력인자를 -1해주었다.

* ### 테스트 2 - 입력이 4일때 IV를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/95953a01cb5022422b7fd66570fb5402751416b6)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/97162c766d85e04d0ac61daeb78a89a962eb8fbf)
    * 반복문 내에서 if문을 이용해서 분기처리했다.
  
* ### 테스트 3 - 입력이 5,6,7,8일때 V,VI,VII,VIII를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/5ba92d822eb3ceeda6e81be200dd0cb646a8fd67)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/07fb096f9be302bc1178f3a20a14d52ba83e35ab)
    * 마찬가지로 반복문을 이용했다. 슬슬 패턴이 잡힌다.

* ### 테스트 4 - 입력이 1666일때 MDCLXVI를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/1bb040e38302d4d9c914cc55a27b07ded80a89bf)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/db3523457306cdacb67f6cd72e19594c76978e7e)
    * 1000, 900, 500, 400 ... 모든 로마표기법에 대한 if문을 만들었다. 문제의 모든 테스트도 통과는 했는데 코드가 좀 더럽다. if/else if문을 사용하는게 별로 안좋다고 "클린 코드"에서 배웠다.
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/b5a3d0f0fafc2fbc4e0b3c3893e085048f238903)
    * TreeMap을 이용했다. TreeMap은 Key들을 정렬해서 저장하는데, 문제는 오름차순 정렬이기 때문에 `map.keySet()`으로 key들을 불러올때 작은 Key값부터 불러온다. 그렇게되면 1000 = M이 아니라 1000 = IIII....이 될것이다. 그래서 내림차순 정렬을 위해 생성자에서 `Comparator.reverseOrder()`를 이용해서 내림차순으로 Key값들을 정렬했다.
    * 반복문을 통해 Key들을 불러오고 불러온 Key값보다 입력 n이 더 크면 key에 대한 로마 표기법 value를 result에 붙여준다.
    * 코드는 확실히 짧아졌다. 그런데 2중for문을 사용해서 처음 코드보다 느린거 같다. 어느게 더 좋다고 할 수 있을까?? 잘 모르겠다..


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Conversion {

    public String solution(int number) {
        String romanNumbers[] = {"M", "CMXC", "CM", "D", "CDXC", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        int arab[] = {1000, 990, 900, 500, 490, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        StringBuilder result = new StringBuilder();
        int i = 0;
        while (number > 0 || arab.length == (i - 1)) {
            while ((number - arab[i]) >= 0) {
                number -= arab[i];
                result.append(romanNumbers[i]);
            }
            i++;
        }
        return result.toString();
    }
}
```

* Map을 배열로 표현한것 외에는 2중 반복문을 사용한것은 같다. best practice가 이거인걸 보면 리팩토링한 코드가 더 좋은 코드가 맞는거라고 생각해도 되겠.....지?
* 다른 코드들을 봐도 if / else if문을 사용한 코드는 없었다.