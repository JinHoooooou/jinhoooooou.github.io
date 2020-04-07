---
title: "Codewars 문제풀기 (03/22)"
excerpt: "Growth of a Population"

categories:
 - codewars
tags:
 - Java
 - codewars
 - coding test
last_modified_at: 2020-03-22
---



## [Growth of a Population](https://www.codewars.com/kata/563b662a59afc2b5120000c6/train/java)

* 초기 인구, 매년 증가 percent, 매년 이주자, 목표량을 인자로 받는다.

* 초기 인구에서 목표량이 되려면 몇 년 필요한지 계산한다.

* ```
  초기인구 1000, 매년 증가 퍼센트 2, 매년 이주자 50, 목표량 1200 
  
  At the end of the first year there will be: 
  1000 + 1000 * 0.02 + 50 => 1070 inhabitants
  
  At the end of the 2nd year there will be: 
  1070 + 1070 * 0.02 + 50 => 1141 inhabitants (number of inhabitants is an integer)
  
  At the end of the 3rd year there will be:
  1141 + 1141 * 0.02 + 50 => 1213
  
  It will need 3 entire years.
  ```


#### 1. Test를 만들었다

* 사실 테스트가 필요한코드인가.. 그냥 단순 계산문제인데.. 라고 생각하긴 했는데 , 일단은 TDD적인 마인드, 습관 기르기를 위해 작성했다.

  * 테스트 코드

  ``` java
    @Test
    public void testShouldReturn3WhenFirstPopulation1000AndPercent2AndNewInhabitant50AndWantToSee1200() {
      //Given
      int firstPopulation = 1000;
      double increasePercent = 2;
      int newInhabitant = 50;
      int expectValue = 1200;
      //When
      int actual = Arge.nbYear(firstPopulation, increasePercent, newInhabitant, expectValue);
      //Then
      assertEquals(3, actual);
    }
  ```
  
  * 실제 코드
  
  ```java
  public class Arge {
  
    public static int nbYear(int p0, double percent, int aug, int p) {
      double result = p0;
      int year = 0;
      while (result < p) {
        result = result + (result * (percent * 0.01)) + aug;
        year++;
      }
  
      return year;
    }
  }
  ```
  
  * 단순 계산 문제라서 바로 Success(약 10분)
  * result사용 하지 않고 p0을 바로 사용할 수 있었는데, p0이 너무 보기싫어서 따로 작성했다 ㅋㅋ...
  

####  2. 답 비교, 느낀점

Best Practice 가장 많이 받은 코드

```java
public class Arge {
    public static int nbYear(int p0, double percent, int aug, int p) {
        int years = 0;
        while (p0 < p) {
            p0 += p0 * percent / 100 + aug;
            years++;
        }
        return years;
    }
}
```

* 메서드 인자가 너무 많다.(클린코드에 위반함)
* 테스트코드를 작성하면서 처음에는 이런 단순 게산문제인데 테스트가 필요한가? 생각했는데 코드를 테스트하는 과정에서 무한루프가 발생했다.
  * 테스트 케이스를 만드는게 이런 버그도 바로바로 찾아낼 수 있어서 좋은거같다.