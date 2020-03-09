---
title: "함수형 프로그래밍과 람다식(1)"
excerpt: "자바 8부터 지원하는 람다에 대한 공부"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-08
---



## Lamda Expression

#### 1. 함수형 프로그래밍

* side effect를 제거하면 프로그램의 동작을 이해하고 예측하는 것이 훨씬 쉬워지기 때문에 side effect가 없는 pure function들로만 이루어져있다.

* side effect란?
  * 변수를 수정하거나 객체 필드 설정하는것
  * 예외처리나 오류나면서 실행중단
  * 콘솔에 출력하거나 사용자 입력값을 읽는것
  * 파일에 쓰거나 읽는것

> 내생각 : 결국 간결한 코드를 위해서 나온 아이디어 같다 객체지향 프로그래밍에서는 모든 동작들을 객체로 구현하는데 간단한 동작들은 함수형 프로그래밍으로 간결하게 해결할 수 있겠다라는 생각이 들었다. 물론 만능은 아닐듯 생략하는것들이 많기때문에?

* 함수형 프로그래밍의 기본 원리들
  1. 변경 불가능한 값을 이용
     * 함수의 계산을 수행하는 동안 변수에 할당된 값들이 변하지 않는다.
     * 변수에 새로운 값을 설정할 수 있을 뿐, 값이 설정되면 바꿀 수 없다.
  2. 함수가 1등 시민
     * 1등 시민이라는 말은 다른 entitiy가 일반적으로 사용할 수 있는 모든 작업들을 지원하는 entity이다.
     * OOP에서는 1등 시민은 객체였다. 변수에 담을 수 있고 인자로 전달될 수 있으며 반환값으로 전달 될 수 있다.
     * 함수형 프로그래밍에서는 함수 자체가 1등 시민이다
  3. 람다와 클로저(Lamda & Clojure)
     * 람다는 익명함수를 말하며 인수의 리스트와 함수 본문만을 가지는 함수이다.
     * 클로저는 자신이 생성될 때의 scope에서 알 수 있었던 변수를 기억하는 함수이다.
  4. 고계함수(Higher-order function)
     * 고계함수는 다른 함수를 인수로 받아들이거나 함수를 리턴하는 함수를 가리킨다.
     * 함수가 정수와 동등한 값으로 다뤄지기 때문에 함수를 인자로 넘기거나 결과 값을 함수로 반환 받을수 있다.
* 함수형 프로그래밍의 컨셉

  1. 변경 가능한 상태를 불변상태로 만들어 side effect를 없앤다.
     * 함수 안에서 상태를 관리하고 상태에 따라 결과 값이 달라지면 안된다는 의미이다.
  2. 모든 것은 객체이다.
     * 클래스 외 함수 또한 객체이기 때문에 함수를 값으로 할당 할 수 있고, 파라미터로 전달 및 결과 값으로 반환이 가능하다.
  3. **코드를 간결하게, 가독성을 높여 로직에 집중시키자**
     * lamda 및 stream같은 api를 통해 보일러 플레이트(변경 없이 계속해서 재 사용할 수 있는 저작품)를 제거하고, 내부에 직접적인 함수 호출을 통해 가독성을 높일 수 있다.
  4. 동시성 작업을 안전하고 쉽게 구현하자



#### 2. Lamda Expression

익명 함수를 이용해서 익명 객체를 생성하기 위한 식

기존 방법 : interface구현 후 interfaceType 할당

람다식 : interfaceType변수를 두고 interface를 implement하는것이아닌 람다식을 이용하여 할당한다.

```java
public interface LamdaInterface1 {
    public void method(String s1, String s2, String s3);
}
//인터페이스는 선언만 되어있음
public class Main {
    public static void main(String[] args) {
        LamdaInterface1 li1 = (String s1, String s2, String s3) -> {System.out.println(s1 + s2 + s3);};
        li1.method("Hello","java","World");
    }
    //클래스로 implement키워드를 이용해 새로 구현하지 않고 바로 사용할 수 있다.
    //매개변수와 실행문만으로 작성(접근자, 반환형 생략)
}
```



```java
public interface LamdaInterface2 {
    public void method(String s1);
}

public class Main {
    LamdaInterface2 li2 = (s1) -> {System.out.println(s1);};
    li2.method("Hello");
    //매개변수가 1개이거나 타입이 같을경우 타입 생략가능
    
    LamdaInterface li3 = s1 -> System.out.println(s1);
    li3.method("Hello");
    //매개변수가 1개이고 실행문이 1개일때 {}, ()생략가능
}
```



```java
public interface LamdaInterface3 {
    public void method();
}

public class Main {
    LamdaInterface3 li4 = () -> {System.out.println("no parameter");};
    li4.method();
    //매개변수가 없다면 ()만 작성
}
```



```java
public interface LamdaInterface4 {
    public int method(int x, int y);
}

public class Main {
    LamdaInterface4 li5 = (x,y) -> return x+y;
    li5.method(10,20);
    //반환값이 있다면 작성
}
```



