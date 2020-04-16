---
title: "Clean Code 읽기(10)"
excerpt: "6장 객체와 자료구조"
classes: wide
categories:
 - Book
tags:
 - clean code
 - refactoring
last_modified_at: 2020-04-16
---



## 6장 객체와 자료구조

* 변수를 private으로 정의하는 이유는 남들이 변수에 의존하지 않게 만들고 싶어서 이다. 그렇다면 왜 많은 프로그래머들은 get함수와 set함수를 당연하게 public으로 선언하여 변수를 외부에 노출시킬까?

### 자료 추상화

* 구체적인 클래스와 추상적인 클래스를 보자

  ```java
  //목록 6-1 구체적인 Point 클래스
  public class Point {
    public double x;
    public double y;
  }
  
  //목록 6-2 추상적인 Point 클래스
  public interface Point {
    double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    double setPolar(double r, souble theta);
  }
  ```

  * 목록 6-1은 구현을 노출한다. 변수를 private으로 선언했지만 get/set 메서드를 제공한다면 구현을 외부로 노출하는 셈이다.
  * 목록 6-2는 클래스가 접근 정책을 강제한다. 좌표를 읽을 때(get)는 각 x,y값을 개별적으로 읽지만, 좌표를 설정할 때(set)는 두 값을 한꺼번에 설정한다.

* 변수 사이에 함수를 넣는다고 구현이 저절로 감춰지지 않는다. 구현을 감추려면 추상화가 필요하다. 

* 추상 인터페이스를 제공하여 사용자가 구현을 모른 채 자료의 핵심을 조작할 수 있어야 진정한 의미의 클래스이다.

* 구체적인 Vehicle 클래스와 추상적인 Vehicle 클래스를 보자

  ```java
  //목록 6-3 구체적인 Vehicle 클래스
  public interface Vehicle {
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
  }
  
  //목록 6-4 추상적인 Vehicle 클래스
  public interface Vehicle {
    double getPercentFuelRemaining();
  }
  ```

  * 목록 6-3은 자동차 연료 상태를 구체적인 숫자 값으로 알려준다.
  * 목록 6-4는 자동차 연료 상태를 백분율이라는 추상적인 개념으로 알려준다.

* 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 편이 좋다. 인터페이스나 get/set 메서드만으로는 추상화가 이뤄지지 않는다. 아무 생각 없이 get/set 함수를 추가하는 방법이 가장 나쁘다.

* 🙋‍♂️ 이해가 잘 안된다. 목록 6-1이랑 목록 6-2 차이가 크게 없는것 같은데, 그냥 get/set 메서드가 있고 없고 차이아닌가..

* 🙋‍♂️ 목록 6-3과 6-4는 그래도 이해가 되긴한다. 근데 충격적인것은 단순히 get/set 메서드를 추가하는게 나쁘다는것이다. 여태 어느 책이나 어느 블로그나 어느 강의를 봐도 get/set 메서드를 추가하곤 했는데, 내가 알고 있던 것이 전면 부정당하는 느낌이라 받아들이기 어렵다... 

### 자료/객체 비대칭

* 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개한다.

* 자료구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다.

* 자료구조와 객체는 서로 상반된다

* 절차적인 도형 클래스를 보자

  ``` java
  public class Square {
    public Point topLeft;
    public double side;
  }
  
  public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
  }
  
  public class Circle {
    public Point center;
    public double radius;
  }
  
  public class Geometry {
    public final double PI = 3.141592653589793;
      
    public double area(Object shape) throws NoSuchShapeException {
      if (shape instance of Square) {
        Sqare s = (Square) shape;
        return s.side * s.side;
      } else if (shape instance of Rectangle){
        Rectangle r = (Rectangle) shape;
        return r.height * r.width;
      } else if (shape instance of Circle){
        Circle c = (Circle) shape;
        return PI * c.radius * c.radius;
      } throws new NoSuchShapeException();
    }
  }
  ```

  * 각 도형 클래스는 아무 메서드도 제공하지 않는 간단한 **자료구조**이다. 도형이 동작하는 방식은 Geometry에서 구현한다.
  * Geometry에 둘레 길이를 구하는 메서드를 추가하더라도 도형 클래스들은 아무 영향도 받지 않는다. 새 도형 클래스를 추가한다면 Geometry에 속한 함수를 모두 고쳐야한다.

* 이번에는 다형적인 도형을 살펴보자

  ```java
  public class Square implements Shape {
    private Point topLeft;
    private double side;
    
    public double area() {
      return side*side;
    }
  }
  
  public class Rectangle implements Shape {
    private Point topLeft;
    private double height;
    private double width;
    
    public double area() {
      return height*width;
    }
  }
  
  publi class Circle implements Shape {
    private Point center;
    private double radius;
    private final double PI = 3.141592653589793;
      
    public double area() {
      return PI * radius * radius;
    }
  }
  ```

  * area()는 다형 메서드이다. 절차적인 도형 클래스와 다르게 Geometry 클래스가 필요없다. 새 도형을 추가해도 기존 함수에 아무 영향을 미치지 않는다. 새 함수를 추가하고 싶다면 도형 클래스 전부 수정해야한다.

* 다시 말하면

  |                         | 새 함수 추가                  | 새 클래스 추가            |
  | ----------------------- | ----------------------------- | ------------------------- |
  | 절차적인 코드(자료구조) | 기존 자료구조 변경없이 가능   | 추가하려면 모든 함수 수정 |
  | 객체지향 코드           | 추가하려면 모든 클래스를 수정 | 기존 함수 변경없이 가능   |

* 객체 지향 코드에서 변경이 어려운 코드는 절차적인 코드에서 쉬우며, 절차적인 코드에서 변경이 어려운 코드는 객체 지향 코드에서 쉽다.
* 모든 것이 객체라는 생각은 미신이다. 때로는 단순한 자료 구조와 절차적인 코드가 가장 적합한 상황도 있다.
* 🙋‍♂️ 다형성에 대해 객체지향 수업에서 배운적이 있다. 그래서 단순히 개념만 알고있었다. 그런데 과제 등을 하며 코드를 짤때 왜 이렇게까지 하는지 이해가 안됐다. 지금은 여러 코드를 보니까 확실히 다형성이 가지는 강점이 보이긴 하는데.. 이걸 어떻게 적용해볼 수 있을까?? 느낌은 있는데 많이 써보질 않아서 어색한점이 있다.
* 🙋‍♂️ 자바 웹 프로그래밍 Next Step 공부를 하며 MVC 패턴중 Controller를 직접 구현하는 실습이 있었는데 확실히 구현할때 하나의 클래스를 수정하려고 모든 코드를 다 볼 필요가 없다는 강점을 느낄 수 있었다.

### 디미터 법칙

* 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙이 디미터 법칙이다.
* 객체는 자료를 숨기고 함수를 공개한다. 즉, get함수로 내부 구조를 공개하면 안된다는 의미이다.
* 🙋‍♂️ 이거 보면서 의아함을 느낀게, 보통 프로젝트할때 DTO클래스(User, Point 등..)마다 get/set 메서드를 만들지않나? 심지어 편하게 구현하려고 lombok이라는 라이브러리도있다.

* "클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야한다"
  * 클래스 C
  * f가 새성한 객체
  * f의 인수로 넘어온 객체
  * C 인스턴스 변수에 저장된 객체

1. 기차 충돌

   * `final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath()` 같이 객차가 한줄로 이어진 기차처럼 보이는 조잡한 코드를 기차 충돌이라고 한다.

     ```java
     Option opts = ctxt.getOptions();
     File scratchDir = opts.getScratchDir();
     final String outputDir = scratchDir.getAbsolutePath();
     ```

   * 조잡한 코드를 나누는것이 좋다. 그런데 ctxt객체가 Options을 포함하며 Options가 ScratchFile을 포함하고, ScratchFile은 또 Absolutepath를 포함한다. 함수 하나가 아는 지식이 매우 많다.
   * 위 클래스가 객체라면 내부 구조를 숨겨야하는데 get함수로 다 노출시키고 있다. 이는 디미터 법칙을 위반한다.
   * 위 클래스가 자료구조라면 당연히 내부 구조를 노출하기 때문에 괜찮다.
     * 자료구조라면 get메서드를 사용하지않고 public 변수로 바꿔 바로 표현하는것이 좋다.

2. 잡종 구조

   * 위 같은 문제로 인해 때때로 절반은 객체, 절반은 자료구조인 잡종구조가 발생한다.
   * 잡종 구조는 중요한 기능을 수행하는 함수도 있고, public 변수나 public get/set 함수도 있다.
   * 이런 잡종 구조는 새로운 함수, 새로운 자료구조도 추가하기 어렵다. 양쪽 단점만 모아놓은 구조이다.
   * 🙋‍♂️ 가장 잘 모르겠는 구조이다. 실제로 대부분 자료 구조의 필드를 private으로 선언하고 public get/set 메서드를 구현하는 형태인데....?

3. 구조체 감추기

   * 위의 ctxt, options, scratchDir이 객체라면 기차 충돌 형태의 코드를 작성하면 안된다. 내부 구조를 감춰야하기 때문이다.
   * ctxt가 객체라면 객체에게 **뭔가를 하라**고 해야지, 가지고 있는 필드를 드러내라고 하면 안된다.

   * 코드 아랫부분을 보면 scratchDirectory의 절대경로를 얻으려는 이유가 임시 파일을 생성하기 때문이다.

     ```java
     String outFile = outputDir = "/" + className.replace('.', '/') + ".class";
     FileOutputStream fout = new FileOutputStream(outFile);
     BufferedOutputStream bos = new BufferedOutputStream(fout);
     ```

   * 그렇다면 get메서드를 통해 scratchDirectory의 경로를 얻지 말고 객체에게 **뭔가를 하라**고 해야하므로 ctxt 객체에게 임시 파일을 만들라고 시키는 것이 더 좋다.

     `BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);`

   * 이렇게 하면 ctxt는 내부 구조를 노출시키지 않고 모듈에서는 여러 객체를 탐색할 필요가 없다.

   * 🙋‍♂️ 확실히 추상화 시키는 방법이 이해가 된다. 전에는 추상화하는게 뭔지 잘 몰랐는데 책을 계속 읽다보니 이해가 된다. 여기서 적용하는것만 실습을 통해 익히면 내것으로 만들 수 있을것 같다.

### 자료 전달 객체 (DTO)

* 자료 구조체의 전형적인 형태는 public 변수만 있고 함수가 없는 클래스이며 이런 자료 구조체를 때로는 DTO(Data Transfer Object)라고 한다.
* DTO는 데이터베이스와 통신하거나 소켓에서 받은 메세지의 구문을 분석할 때 유용하다. 흔히 데이터베이스에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 가장 처음 사용하는 구조체이다.

1. 활성 레코드

   ```java
   public class Address {
     private String street;
     private String streetExtra;
     private String city;
     private String state;
     private String zip;
       
     public Address(String street, String streetExtra, String city, String state, String zip) {
       this.street = street;
       this.streetExtra = streetExtra;
       this.city = city;
       this.state = state;
       this.zip = zip;
     }
       
     public String getStreet() {return street;}
     public String getStreetExtra() {return streetExtra;}
     public String getCity() {return city;}
     public String getState() {return state;}
     public String getZip() {return zip;}    
   }
   ```

   * 활성 레코드는 DTO의 특수한 형태이다. public 변수가 있거나 private 변수에 get/set 메서드가 있는 자료 구조이다.
   * 활성 레코드는 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과이다.
   * 활성 레코드에 비즈니스 로직 메서드를 추가하여 자료 구조를 객체로 취급하는 개발자가 많다. 활성 레코드는 자료구조로 취급한다. 비즈니스 로직을 담으면서 내부 자료를 숨기는 객체는 따로 생성한다.
   * 🙋‍♂️ 위에서도 말했지만 이런 구조를 흔히 본다. 객체로 취급하는 개발자가 많다가 아니라 거의 다 이러지않나? 뭐.. 내가 현업 프로젝트 코드를 보지못해서 잘 모르겠는데, 아마 대학생 수준에서는 이런식으로 구현 안하는 사람이 없을텐데.. 그래서 더 어렵다.

