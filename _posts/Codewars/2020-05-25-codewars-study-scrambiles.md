---
title: "Codewars 문제풀기 (05/25)"
excerpt: "Scramblies (5kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-25
---



# [Scramblies](https://www.codewars.com/kata/55c04b4cc56a697bb0000048)

* String 2개를 입력으로 받는다.

* 첫번째 인자 String을 재배열해서 두번째 인자 String을 만들 수 있는지 체크하는 메서드를 만들어라.

* 입력 String은 only lowercase letter이고 기호나 숫자는 포함하지않는다.

  ``` 
  scramble("rkqodlw", "world") 👉 world를 만들 수 있다(worldkq) 👉 true
  scramble("cedewaraaossoqqyt", "codewars") 👉 true
  scramble("katas", "steak") 👉 'e'가 없기 때문에 만들 수 없다 👉 false
  ```
  
  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력 인자가 "rkqodlw", "world" 일 때 True이다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/3b6cfd0f07ad090519ceb67bd7d142bec3b33135)
  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/f817c8b0f1ee7ff695123ebec1b809076ee0fce1)
    * str1에서 한 문자씩 추출해서 str2에 있는지 확인하고 있으면 삭제한다. for문을 다 돌고 str2의 길이가 0이면 만들 수 있다는 뜻이므로 true를 리턴한다.
  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/1b60ebb643f267c8ce411675dcff070cb4833ac9)
    * 테스트 코드의 given을 then에 묶는게 더 깔끔해서 수정했다.
    * `char target`을 `String target`으로 수정하고 마지막에 `trim()`메서드도 삭제했다.

* ### 테스트 2 - 입력 인자가 "katas", "steak"일 때 False이다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/9f6a1de617e7cdec558e0f4e89e5ee38ea24333d)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/51812bf8ed1c5f433781d0aa46bee0f3a50f28e7)
    * 코드는 그대로인데, for문을 조금 수정했다.


## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Scramblies {
    
    public static boolean scramble(String str1, String str2) {
        if (str2.length() > str1.length()) return false;
        for (String s: str2.split("")) {
          if (!str1.contains(s))  return false;
          str1 = str1.replaceFirst(s,"");
        }        
       
        return true;
    }
}
```

* str1의 문자들로 str2를 재배열이 아닌, str2의 문자들이 str1에 있는지 확인해서 true, false를 리턴했다. 