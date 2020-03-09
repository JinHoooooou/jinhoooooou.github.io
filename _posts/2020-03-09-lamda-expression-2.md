---
title: "함수형 프로그래밍과 람다식(2)"
excerpt: "이펙티브 자바(Effective Java) 7장"

categories:
 - Blog
tags:
 - Java
 - Codewars
last_modified_at: 2020-03-08
---



## 이펙티브 자바 (Effective Java) 7장 람다와 스트림

#### 아이템 42 익명 클래스 보다는 람다를 사용해라

* 이전 자바 - 추상 메서드를 하나만 담은 인터페이스 사용

* 자바 8 이후 - **추상 메서드 하나짜리 인터페이스(함수형 인터페이스)**를 람다식을 통해 인스턴스를 만들 수 있게 되어 간결한 코드 작성 가능

  ```java
  //익명클래스 사용 --> 코드가 너무 길기 때문에 함수형 프로그래밍이 적합하지 않았음
  Collections.sort(words, new Comparator<String>() {
      public int compare(String s1, String s2) {
          return Integer.sompare(s1.length(), s2.length());
      }
  });
  
  //람다식 사용 --> 훨씬 간결한 코드
  Collections.sort(words, (s1, s2) -> Integer.compare(s1.length(), s2.length());                    
  ```

  * 람다, 매개변수(s1,s2), 반환값의 타입은 각 `Comparator<String>`, `String` `int`지만 컴파일러가 추론할 수 있기 때문에 생략이 가능하다. 타입을 확실하게 명시해야 할 경우외에는 타입을 생략해도된다.
  * 비교자 생성 메소드를 사용해 `Collections.sort(words, comparingInt(String::length));`로 더 간결하게 표현가능

* Operation 열거 타입 예시

  ```java
  //상수별로 클래스 몸체와 데이터를 사용한 열거 타입
  public enum Operation {
      PLUS("+") {
          public double apply(double x, double y) {return x+y;}
      },
      MINUS("-") {
          public double apply(double x, double y) {return x-y;}
      },
      TIMES("*") {
          public double apply(double x, double y) {return x*y;}
      },
      DIVIDE("/") {
          public double apply(double x, double y) {return x/y;}
      };
      private final String symbol;
      Operation(String symbol) {this.symbol = symbol;}
      @Override public String toString(){return symbol;}
      public abstract double apply(double x, double y);
  }
  
  //람다를 인스턴스 필드에 저장해 상수별 동작을 구현한 열거 타입
  public enum Operation {
      PLUS ("+" (x,y) -> x+y),
      MINUS ("-" (x,y) -> x-y),
      TIMES ("*" (x,y) -> x*y),
      DIVIDE ("/" (x,y) -> x/y);
      
      private final String symbol;
      private final DoubleBinaryOperator op;
      
      Operation(String symbol, DoubleBinarayOperator op) {
          this.symbol = symbol;
          this.op = op;
      }
      @Override public String toString() {return symbol;}
      public double apply(double x, double y) {
          return op.applyAsDouble(x,y);
      }
  }
  ```

  * 두 코드를 비교해보면 람다식을 사용한 코드가 상수별 동작 코드를 훨씬 간결하게 표현했다.

  * 근데 `String symbol`외에 `DoubleBinaryOperator op` 필드가 추가되었고, `apply`메소드가 추상메서드가 아니다.  상수의 동작을 나타내는 람다식을 `DoubleBinaryOperator`라는 인터페이스에 할당한것이다. 이는 `java.util.function`패키지가 제공하는 함수 인터페이스 중 하나이다.

  * **람다는 이름도 없고 문서화도 못한다. 따라서 코드 자체로 동작이 명확히 설명이 불가능하거나 코드 줄 수 가 많아지면 쓰지 말아야한다.**

  * enum 생성자의 인수들의 타입도 컴파일 타임에 추론되기 때문에 enum 생성자 안의 람다는 enum 인스턴스 멤버에 접근할 수 없다(인스턴스는 런타임에 만들어지기 때문) 따라서 상수별 동작을 몇 줄로 구현할 수 없거나, 인스턴스나 메소드를 사용해야하는 상황에는 람다식보다는 상수별 클래스 몸체를 사용해야한다.

    > 잘 이해가 안되서 enum 생성자에 대해 찾아봄  [http://www.nextree.co.kr/p11686/](http://www.nextree.co.kr/p11686/)

  * 추상 클래스의 인스턴스, 추상 메서드가 여러개인 인터페이스의 인스턴스를 만들 때 람다를 쓸 수 없으며, 자기 자신을 참조 할 수 없다.

#### 아이템 43 람다보다는 메서드 참조를 사용하라

* 람다 쓰는 이유는 간결함이다. 람다보다 더 간결하게 만드는 방법이 있는데 **메서드 참조(method reference)**이다. 

  ``` java
  //람다를 사용한 코드
  map.merge(key, 1, (count, incr) -> count + incr);
  
  //메서드 참조를 사용한 코드
  map.merge(key, 1, Integer::sum);
  ```

  * merge 메서드는 키, 값, 함수를 인수로 받으며, 주어진 키가 맵 안에 아직 없다면 주어진 {키, 값} 쌍을 그대로 저장하고 키가 있다면 함수를 현재 값과 주어진 값에 적용한 다음 그 결과로 현재 값을 덮어쓴다. 즉, {키, 함수의 결과}쌍을 저장한다.
  * 람다로 표현하면 깔끔하지만 count, incr는 크게 하는 일 없이 공간을 차지하며 단순히 합을 반환한다.
  * 자바 8부터 Integer 클래스의 static method sum을 제공하여 더 깔끔하게 작성할 수 있다.
  * 코드가 짧다고 능사가아니라 의도를 표현할 수 있어야한다(이건 클린코드내용과 비슷한듯) 즉, 메서드 참조가아닌 매개변수로 의도를 더 명확히 드러낼 수 있으면 람다를 사용하는것이 더 좋을수도있다. 더 확장하면 람다쓰지않고 익명클래스 사용하는것이 더 좋을수도 있을것 같다.

* 메서드 참조의 유형

  * | 메서드 참조 유형   | 예                     | 같은 기능을 하는 람다                                 |
    | ------------------ | ---------------------- | ----------------------------------------------------- |
    | 정적               | Integer::parseInt      | str -> Integer.parseInt(str)                          |
    | 한정적(인스턴스)   | Instant.now()::isAfter | Instant then = Instant.now();<br />t->then.isAfter(t) |
    | 비한정적(인스턴스) | String::toLowerCase    | str->str.toLowerCase()                                |
    | 클래스 생성자      | TreeMap<K,V>::new      | () -> new TreeMap<K,V>()                              |
    | 배열 생성자        | int[]::new             | len -> new int[len]                                   |

#### 아이템 44 표준 함수형 인터페이스를 사용하라

* 자바가 람다를 지원하면서 상위 클래스의 메서드를 재정의하여 원하는 동작을 구현하는 템플릿 메서드 패턴의 매력이 줄어들었다. 오늘날은 같은 효과의 함수 객체를 받는 정적 팩터리나 생성자를 제공하는 것이다.

* LinkedHashMap 클래스의 protected 메서드인 removeEldestEntry를 재정의하여 캐시로 사용할 수 있다.

  ```java
  protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
      return size() > 100;
  }
  
  //불필요한 함수형 인터페이스
  @FuntionalInterface interface EldestEntryRemovalFunction<K,V> {
      boolean remove(Map<K,v> map, Map.Entry<K,v> eldest);
  }
  ```

  * removeEldestEntry는 size()를 호출하여 맵의 원소 수를 알아내는데, removeEldestEntry가 인스턴스 메서드라서 가능한 방식이다. 하지만 생성자에 넘기는 함수 객체는 이 맵의 인스턴스 메서드가 아니다. 팩터리나 생성자를 호출할 때는 맵의 인스턴스가 존재하지 않기 때문이다. 따라서 맵은 자기 자신 함수도 객체에 건네줘야 한다.  그래서 remove 메서드의 인자로 `Map<K,V> map`이 포함되어있다.

* 불필요한 함수형 인터페이스를 만들 필요는 없다. 표준 함수형 인터페이스를 활용하면된다. `BiPredicate<Map<K,V>, Map.Entry<K,V>`를 사용하면 된다.

* java.util.function패키지에 43개의 인터페이스가 있다. 전부 익히는건 어렵지만 기본 인터페이스 6개만 익힌다면 나머지는 유추 할 수 있다.

  | 인터페이스        | 함수 시그니처       | 예                  |
  | ----------------- | ------------------- | ------------------- |
  | UnaryOperator<T>  | T apply(T t)        | String::toLowerCase |
  | BinaryOperator<T> | T apply(T t1, T t2) | BigInteger::add     |
  | Predicate<T>      | boolean test(T t)   | Collection::isEmpty |
  | Function<T,R>     | R apply(T t)        | Arrays::asList      |
  | Supplier<T>       | T get()             | Instant::now        |
  | Consumer          | void accept(T t)    | System.out::println |

  * Operator : 인수가 1개인 Unary 2개인 Binary이고, 인수의 타입과 반환형 타입이 같다
  * Predicate : 인수 하나를 받아 boolean 타입을 반환한다.
  * Function : 인수와 반환 타입이 다른 함수
  * Supplier : 인수를 받지 않고 값만 반환한다.
  * Consumer : 인수를 받고 반환 값이 없다.
  * 참조 : [https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)

* 표준 함수형 인터페이스 대부분은 기본 타입만 지원한다.

* 대부분은 함수형 인터페이스를 작성하는것 보다 표준 함수형 인터페이스를 사용하는 편이 좋다. 그렇다면 직접 작성할 때는 언제일까? 셋 중 하나만 만족해도 직접 구현해야 할지 고민해봐야한다.

  * 자주 쓰이며, 이름 자체가 용도를 명확하게 설명한다.
  * 반드시 따라야 하는 규약이 있다.
  * 유용한 디폴트 메서드를 제공할 수 있다.

* 직접 만든 함수형 인터페이스에는 항상 `@FunctionalInterface` 에너테이션을 사용한다.
  * 해당 클래스의 코드나 설명 문서를 읽을 이에게 인터페이스가 람다용으로 설계된 것임을 알려준다.
  * 해당 인터페이스가 추상 메서드를 오직 하나만 가지고 있어야 컴파일 되게 해준다.
  * 그러므로 유지보수 과정에서 누군가 메서드를 추가하지 못하게 한다.

> 내생각 : 아직까지는 인터페이스를 직접 설계하는 수준까지는 아니기 때문에 표준 함수형 인터페이스를 얼마나 잘 활용할 수 있을까가 더 관건이라고 생각된다. 표준 함수형 인터페이스를 계속 외운다기보다는 필요할때 잘 쓸 수 있도록 자주 찾아보는 식으로 숙달 할 수 있도록 노력해야겠다.

---

#### 추가로 공부해야 할것 

* abstract --> 추상 메소드, 추상 클래스
* interface --> 추상 클래스와 인터페이스의 차이

* 추상 클래스와 인터페이스 공통점 - 선언만 있고 구현 내용이 없는 클래스
* 선언만 있기 때문에 추상클래스,인터페이스만으로 인스턴스를 생성할 수 없다.
* 추상클래스 extends로 상속을 받거나 인터페이스 implements를 이용해 구현 클래스로 객체생성할수있다

* 차이점은?

* 목적의 차이이다

  * 추상클래스는 공통적인 기능을 하는 객체들의 초상화이다 (사자, 고양이, 고릴라, 돌고래 --> 공통기능은 포유류 --> 추상클래스 포유류 --> 나머지는 상속받는 자식클래스)

  * 인터페이스는 구현하는 모든 클래스에 대해 특정한 메서드가 반드시 존재하도록 강제하는 역할

  * 인터페이스는 다중상속이가능

  * but 다중상속 가능 여부가 포인트가아닌 목적이 다른것이 포인트

  * 그니깐 총정리를 하면,

    인터페이스는 다형성이라 생각하면되고,

    상속은 부모 - 자식 관계.. 

    부모가 갖고있는 기능을 유전 받으면서, 기능을 더 추가한다거나, 

    부모의 유전된 기능을 약간 수정할 때 쓴다.

  [https://jeong-pro.tistory.com/82](https://jeong-pro.tistory.com/82)

  [https://marobiana.tistory.com/58](https://marobiana.tistory.com/58)