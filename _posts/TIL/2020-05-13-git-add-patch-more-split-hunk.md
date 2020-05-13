---
title: "git add --patch의 e 옵션"
excerpt: "커밋 단위를 쪼갤 때 더 작게 쪼개는 방법에 대해 알아보자"
classes: wide
categories:
 - TIL
tags:
 - Git
 - TIL
last_modified_at: 2020-05-13
---



# 1. git add -p

```java
public class Kata {

  public static String expandedForm(int num) {
    int[] numbers = new int[(int) Math.log10(num) + 1];
    for (int i = 0; i < numbers.length; i++) {
      numbers[i] = num % 10;
      num /= 10;
    }
    String result = "";
    for (int i = numbers.length - 1; i >= 0; i--) {
      if (numbers[i] != 0) {
        result += "" + (numbers[i] * (int) Math.pow(10, i)) + " + ";
      }
    }
    return result.substring(0, result.length() - 3);
  }
}
```

* 코드를 다 작성하고 나는 `String result ="";` 를 기준으로 위는 "배열에 number들을 저장" 이라는 커밋과 아래는 "result String 만들기"라는 커밋으로 구분 지어 하고싶다.

* 그러나 평소처럼 git add Kata.java를 하면 전체 코드가 스태이징 되기 때문에 그럴 수가 없다 그러면 어떻게 할까?

* 무식하게 한다면 `String result = "";`부분 아래 코드를 임시로 지운 후 "배열에 number들을 저장"커밋을 남기고, 그 뒤에 다시 아래 코드를 붙인 후 "result String 만들기"라는 커밋을 남길 것이다. 근데 이 방법은 매우 무식하다.

* 그래서 git add -p로 쪼개보자

  ```
  User@LEEJINHO MINGW64 ~/Desktop/kw/codeWars (master)
  $ git add -p src/writeNumberInExpandedForm_20200513/KataBestPractice.java
  ...
  @@ -1,4 +1,20 @@
   package writeNumberInExpandedForm_20200513;
  
   public class KataBestPractice {
  +
  +  public static String expandedForm(int num) {
  +    int[] numbers = new int[(int) Math.log10(num) + 1];
  +    for (int i = 0; i < numbers.length; i++) {
  +      numbers[i] = num % 10;
  +      num /= 10;
  +    }
  +    String result = "";
  +    for (int i = numbers.length - 1; i >= 0; i--) {
  +      if (numbers[i] != 0) {
  +        result += "" + (numbers[i] * (int) Math.pow(10, i)) + " + ";
  +      }
  +    }
  +    return result.substring(0, result.length() - 3);
  +  }
  +
   }
  Stage this hunk [y,n,q,a,d,e,?]? 
  ```

  * patch 옵션은 다음과 같다
    * y - stage this hunk
    * n - do not stage this hunk
    * q - quit; do not stage this hunk or any of the remaining ones
    * a - stage this hunk and all later hunks in the file
    * d - do not stage this hunk or any of the later hunks in the file
    * e - manually edit the current hunk
    * g - select a hunk to go to
    * / - search for a hunk matching the given regex
    * j - leave this hunk undecided, see next undecided hunk
    * J - leave this hunk undecided, see next hunk
    * k - leave this hunk undecided, see previous undecided hunk
    * K - leave this hunk undecided, see previous hunk
    * s - split the current hunk into smaller hunks
    * e - manually edit the current hunk
    * ? - print help

* 이제 add 할 부분을 쪼개기 위해 s를 눌러보면 다음과 같이 나온다.

  ```
  Stage this hunk [y,n,q,a,d,e,?]? s
  Sorry, cannot split this hunk
  @@ -2,4 +2,19 @@ package writeNumberInExpandedForm_20200513;
  
   public class KataBestPractice {
  
  +  public static String expandedForm(int num) {
  +    int[] numbers = new int[(int) Math.log10(num) + 1];
  +    for (int i = 0; i < numbers.length; i++) {
  +      numbers[i] = num % 10;
  +      num /= 10;
  +    }
  +    String result = "";
  +    for (int i = numbers.length - 1; i >= 0; i--) {
  +      if (numbers[i] != 0) {
  +        result += "" + (numbers[i] * (int) Math.pow(10, i)) + " + ";
  +      }
  +    }
  +    return result.substring(0, result.length() - 3);
  +  }
  +
   }
  Stage this hunk [y,n,q,a,d,e,?]? 
  ```

  * `Sorry, cannot split this hunk`라며 쪼개지지 않는다. 그러면 그냥 무식하게 코드 지웠다가 다시 붙여야할까? 아니다!



* 옵션 e를 통해 더 작게 쪼개보자

  ```
  # Manual hunk edit mode -- see bottom for a quick guide.
  @@ -2,4 +2,19 @@ package writeNumberInExpandedForm_20200513;
  
   public class KataBestPractice {
  
  +  public static String expandedForm(int num) {
  +    int[] numbers = new int[(int) Math.log10(num) + 1];
  +    for (int i = 0; i < numbers.length; i++) {
  +      numbers[i] = num % 10;
  +      num /= 10;
  +    }
  +    String result = "";
  +    for (int i = numbers.length - 1; i >= 0; i--) {
  +      if (numbers[i] != 0) {
  +        result += "" + (numbers[i] * (int) Math.pow(10, i)) + " + ";
  +      }
  +    }
  .git/addp-hunk-edit.diff [unix] (14:51 13/05/2020)                                                                                       1,1 꼭대기
  ```

  * 옵션 e를 선택하면 스태이징 할 부분을 수정할 수 있다. +되어있는 부분을 삭제하여 원하는 부분만 스태이징 할 수 있다.

    



