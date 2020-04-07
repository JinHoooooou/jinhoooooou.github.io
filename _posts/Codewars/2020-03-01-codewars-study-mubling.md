---
title: "Codewars 문제풀기 (03/01)"
excerpt: "Mumbling"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-01
---



## [Mumbling](https://www.codewars.com/kata/5667e8f4e3f572a8f2000039/train/java)

* 문자열을 인자로 받아서 결과 문자열 출력
* accum("abcd") --> "A-Bb-Ccc-Dddd"
* accum("RqaEzty") --> "R-Qq-Aaa-Eeee-Zzzzz-Tttttt-Yyyyyyy"

* 문자열은 a-z A-z로만 이루어져있다.

  

#### 1. Test를 만들었다

* 문자열길이가 1일때

  입력한문자 하나의 대문자를 출력해주면되겠구나

  ```java
  public static String accum(String s){
      return s.toUpperCase();
  }
  ```

* 문자열길이가 2이상일때

  문자열의 index+1만큼 문자를 찍어내되 첫문자는 대문자, 그 이후는 소문자 문자사이는 "-"로 이어준다. 라고생각

  ```java
  public static String accum(String s){
      String result = "";
      for(int i = 0; i < s.length(); i++) {
          for(int j = 0; j < i + 1 ; j++) {
              if(j == 0) {
                  result += String.valueOf(s.charAt(i)).toUpperCase();
              }
              else {
                  result += String.valueOf(s.charAt(i)).toLowerCase();
              }
          }
          result += "-";
      }
      return result;
  }
  ```

  제출하니까 문자열 마지막에 "-"가 붙어서 fail이 떴다. 그래서 수정

  ```java
  public static String accum(String s){
      String result = "";
      for(int i = 0; i < s.length(); i++) {
          for(int j = 0; j < i + 1 ; j++) {
              if(j == 0) {
                  result += String.valueOf(s.charAt(i)).toUpperCase();
              }
              else {
                  result += String.valueOf(s.charAt(i)).toLowerCase();
              }
          }
          if(i == s.length() - 1) {
          	break;
          }
          result += "-";
      }
      return result;
  }
  ```

  제출하니까 Success.



#### 2. 리팩토링 시작

코드를 보니까 뭘 수정할 수 있을까 고민해보니 첫 문자는 어짜피 대문자니까 바깥 for문에 넣어줘도 되겠다고 생각했다.

```java
public static String accum(String s){
    String result = "";
    for(int i = 0; i < s.length(); i++) {
        result += String.valueOf(s.charAt(i)).toUpperCase();
        for(int j = 0; j < i ; j++) {
            result += String.valueOf(s.charAt(i)).toLowerCase(); 
        }
        if(i == s.length() - 1) {
        	break;
        }
        result += "-";
    }
    return result;
}
```

`if(i == s.length() - 1) break; `이 부분을  없애서 더 깔끔하게 만들어주면 좋을거같은데 생각은 들었는데.. 더 좋은방법이 생각나지않아서 그냥 제출



#### 3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Accumul {
  public static String accum(String s) {
    StringBuilder bldr = new StringBuilder();
    int i = 0;
    for(char c : s.toCharArray()) {
      if(i > 0) bldr.append('-');
      bldr.append(Character.toUpperCase(c));
      for(int j = 0; j < i; j++) bldr.append(Character.toLowerCase(c));
      i++;
    }
    return bldr.toString();
  }
}
```

* 바깥 for문에서 s.toCharArray로 해결했구나, 이렇게하면 확실히 `if(i == s.length() - 1) break; `이부분 없애줄 수 있구나 느낌
* 근데 String써도 될거같은데 왜 StringBuilder를 쓰지? 그러고보면 String StringBuilder StringBuffer 차이점이뭘까?
* inline으로 표현하니 확실히 가독성이 떨어지는 걸 느낀다. 적어도 이 코드에선 그렇게 느낌
* toUpperCase, toLowerCase가 String클래스뿐 아니라 Character클래스에도 내부 메소드로 존재하는구나



Best Practice 두번째로 많이 받은 코드

```java
public class Accumul {

    public static String accum(String s) {
        s = s.toLowerCase();
        StringBuilder builder = new StringBuilder();

        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            builder.append(Character.toUpperCase(ch));
            for (int j = 0; j < i; j++) {
                builder.append(ch);
            }
            builder.append("-");
        }

        return builder.deleteCharAt(builder.lastIndexOf("-")).toString();
    }
}
```

* 이게 그나마 내가 작성한 코드랑 가장 비슷하다.

* `deleteCharAt(builder.lastIndexOf("-"))`이걸로 마지막 "-"를 지워주면 되겠는구나!

* 여기도 StringBuilder를 썼네?

  

#### 4. 궁금한거 검색

* String과 StringBuilder, StringBuffer의 차이

  * String은 불변객체이므로 새로운 값을 할당할때마다 새로 생성된다. String 클래스가 생성될때 내부 변수와 메서드들도 같이 생성되며 각 String의 주소값이 stack에 쌓이고 클래스들은 garbage collector가 호출되기전까지 heap에 쌓이므로 많이 사용될수록 메모리 관리측면에서 치명적이다.

  * StringBuilder와 StringBuffer는 객체 생성을 한번만 하고 append를 이용해 메모리 값을 변경시켜 문자열을 변경한다. 

  * 따라서 String은 문자열을 단순히 읽는 작업을 할때 좋고, StringBuilder와 StringBuffer는 문자열 연산이 자주있을때 사용하면 좋다.

* StringBuilder와 StringBuffer의 차이 

  * StringBuffer는 멀티 쓰레드 환경에서 동기화가 가능하지만 StringBuilder는 동기화를 지원하지 않는다.
  * 따라서 싱글 쓰레드환경에서는 StringBuilder가 적합하고, 멀티 쓰레드 환경에서는 StringBuffer가 적합하다.

* lastIndexOf와 indexOf차이

  ```java
  String test = "A-Bb-Ccc-Ddddd-Eeeeee";
  int indexOf = test.indexOf("-"); // 1
  int lastIndexOf = test.lastIndexOf("-"); // 14
  ```

  indexOf는 찾으려는 문자열을 왼쪽에서부터 찾는다. "-" 문자열은 [1] index이므로 1을 반환

  lastIndexOf는 찾으려는 문자열을 오른쪽에서부터 찾는다. "-" 문자열은 [14] index이므로 14를 반환