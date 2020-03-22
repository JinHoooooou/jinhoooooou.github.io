---
title: "Codewars 문제풀기 (03/22)"
excerpt: "Take a Ten Minute Walk"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-22
---



## [Take a Ten Minute Walk](https://www.codewars.com/kata/54da539698b8a2ad76000228/train/java)

* char 배열을 인자로 받는다.
* 'e', 'w', 's', 'n'에 따라 동 서 남 북으로 움직인다.
* 10번 움직였을 때 위치가 제자리라면 true를 리턴한다.
* TenMinWalk.isValid(new char[] {'n','s','n','s','n','s','n','s','n','s'})) --> 제자리로 왔으므로 true
* TenMinWalk.isValid(new char[] {'w','e','w','e','w','e','w','e','w','e','w','e'})); -->10번 넘게 움직였으므로 false
* TenMinWalk.isValid(new char[] {'n','n','n','s','n','s','n','s','n','s'})); --> 10번 움직였으나 제자리가 아니므로 false


#### 1. Test를 만들었다

* 배열 길이가 10이 아닐 때 false를 리턴해야한다.

  * 테스트 코드 (`assertFasle()`, `assertTrue`를 쓸 수 있지만 문제들 통일성을 위해 `assertEquals()`사용)

  ``` java
    @Test
    public void testShouldReturnFalseWhenInputLengthIsNotTen() {
      //Given : Set walking count less then 10
      char[] given = new char[5];
      //When : Call isValid method
      boolean actual = TenMinWalk.isValid(given);
      //Then : Should return false
      assertEquals(false, actual);
    }
  ```
  
  * 실제 코드
  
  ```java
  public class TenMinWalk {
  
    public static boolean isValid(char[] walk) {
      return walk.length == 10;
    }
  }
  
  ```
  
* 배열 길이가 10이지만, 다 움직이고 제자리에 오지 않을경우 false를 리턴해야한다.

  * 테스트 코드

  ```java
    @Test
    public void testShouldReturnFalseWhenWalkerNotComeStartingPoint() {
      //Given : Set walking count 10, not coming to starting point
      char[] given = {'w', 's', 'e', 'n', 'w', 'w', 'e', 'n', 'w','w'};
      //When : Call isValid method
      boolean actual = TenMinWalk.isValid(given);
      //Then : Should return false
      assertEquals(false, actual);
    }
  ```

  * 실제 코드

  ```java
  public class TenMinWalk {
  
    public static boolean isValid(char[] walk) {
      if (walk.length != 10) {
        return false;
      }
      int wCount = 0;
      int nCount = 0;
      int sCount = 0;
      int eCount = 0;
      int invalidCount = 0;
      for (int i = 0; i < walk.length; i++) {
        switch (walk[i]) {
          case 'w':
            wCount++;
            break;
          case 'n':
            nCount++;
            break;
          case 'e':
            eCount++;
            break;
          case 's':
            sCount++;
            break;
          default:
            invalidCount++;
            break;
        }
      }
      if (wCount == eCount && nCount == sCount) {
        return true;
      }
      return false;
    }
  }
  
  ```

  * 위, 아래 / 좌, 우 쌍으로 이동 횟수가 같아야 결국 제자리로 돌아오기 때문에 각 count를 세고 같은지 판단하여 true, false를 리턴하도록 풀었다.

* 배열의 길이가 10이고, 다 움직이고 제자리로 돌아온 경우에 true를 리턴해야한다.

  * 테스트 코드

  ```java
    @Test
    public void testShouldReturnTrueWhenWalkerComeStartingPoint() {
      //Given : Set walking count less then 10
      char[] given = {'w', 's', 'e', 'n', 'w',
          's', 'e', 'n', 'w','e'};
      //When : Call isValid method
      boolean actual = TenMinWalk.isValid(given);
      //Then : Should return true
      assertEquals(true, actual);
  ```
  
  * 실제 코드 그대로
  
    * Success(약 20분)



#### 2. 리팩토링

`stream.filter()`를 사용하면 switch문이 필요없지 않을까 해서 사용해봤다.

```java
public class TenMinWalk {

  public static boolean isValid(char[] walk) {
    if (walk.length != 10) {
      return false;
    }

    long nCount = String.valueOf(walk).chars().filter(character -> character == 'n').count();
    long sCount = String.valueOf(walk).chars().filter(character -> character == 's').count();
    long wCount = String.valueOf(walk).chars().filter(character -> character == 'w').count();
    long eCount = String.valueOf(walk).chars().filter(character -> character == 'e').count();
    return wCount == eCount && nCount == sCount;
  }
}

```

* 변수 선언 없이 그냥 return에 적어주려 햇는데 너무 길어지고 지저분해 보여서 변수 할당해주었다.



####  3. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class TenMinWalk {
  public static boolean isValid(char[] walk) {
    if (walk.length != 10) {
      return false;
    }
    int x = 0, y = 0;
    for (int i = 0; i < 10; i++) {
      switch (walk[i]) {
        case 'n':
          y++;
          break;
        case 'e':
          x++;
          break;
        case 's':
          y--;
          break;
        case 'w':
          x--;
          break;
      }
    }
    return x == 0 && y == 0;
  }
}
```

* switch/case문을 사용하는것보다 Stream을 사용하는것이 더 깔끔해보여서 사용했는데 Best Practice는 오히려 이 방법이 더 많이 받았다. 그리고 나는 w, e, s, n count를 모두 세었는데 여기서는 x, y좌표로 ++, -- 해주었다.  변수 선언을 덜 해서 좋은것 같다.
* 문제가 좀 길어서 잘못 이해했다. 처음에 그냥 10번 count에 맞게 이동만 하면 true를 return한다고 생각해서 그렇게 풀다가 마지막 테스트를 보고 제자리로 돌아와야한다는 것을 알았다. 다시 한번 테스트 코드의 중요성을 알게되었다..