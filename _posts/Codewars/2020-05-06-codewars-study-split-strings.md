---
title: "Codewars 문제풀기 (05/06)"
excerpt: "Split Strings (6kyu)"
classes: wide
categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-05-06
---



# [Split Strings](https://www.codewars.com/kata/515de9ae9dcfc28eb6000001/train/java)

* String을 인자로 받는다.

* 인자를 2문자 씩 쪼갠 String 배열을 리턴한다.

* String 길이가 홀수라면 끝에 _를 붙인다.

  ``` 
  StringSplit.solution("abcdef") 👉 {ab, cd, ef}
  StringSplit.solution("abcde") 👉 {ab, cd, e_}
  ```

  

## 1. Test와 리팩토링

* ### 테스트 1 - 입력이 "abcdef"라면 {ab, cd, ef}를 리턴한다.

  * [테스트 코드, 실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/9bec59885825cb73fa5ab4a80eec6ec6ecc29b14)

  
  - [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/b16b0cdfe32d8cd81329fd2dba7bece0c37db616) - for문을 이용해서 String을 2문자씩 자르고 배열에 저장
  
    
  
* ### 테스트 2 - 입력이 "abcde"라면 {ab, cd, e_}를 리턴한다.

  * [테스트 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/084cb10d285cec0088981558db211f00e74b9ddf)

  * [실제 코드](https://github.com/JinHoooooou/codeWarsChallenge/commit/2f35e93a7405b95a78f9753d19bc4023de02e8fe) - 입력 string 길이가 홀수라면 끝에 _를 붙여준다. 

  * [리팩토링](https://github.com/JinHoooooou/codeWarsChallenge/commit/748826e6711e6f5d974c02fdeed66487c6deb440) - 지역변수 target 삭제

    

---

## 2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class StringSplitBestPractice {

  public static String[] solution(String s) {
    s = (s.length() % 2 == 0) ? s : s + "_";
    return s.split("(?<=\\G.{2})");
  }
}
```

* 정규식을 사용했는데 \G가 무엇을 나타내는지 모르겠다 그래서 [정규식 테스트 사이트](https://regex101.com/)에서 확인해봤는데

  > \G asserts position at the end of the previous match or the start of the string for the first match

라고 한다. 문자열을 2개씩 쪼갤 때 이방법을 쓴다고 알아두는게 좋을것같다..