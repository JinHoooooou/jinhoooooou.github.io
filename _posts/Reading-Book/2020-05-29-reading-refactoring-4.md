---
title: "[Reading Book]Refactoring(4)"
excerpt: "6장 메서드 정리"
classes: wide
categories:
 - Book
tags:
 - clean code
 - refactoring
last_modified_at: 2020-05-29
---



# 6장 메서드 정리 (Composing Methods)

* 리팩토링의 많은 부분이 메서드를 정리해서 코드를 적정하게 포장하는 것이다.
  * 대부분의 문제는 지나치게 긴 메서드에서 나온다. 복잡한 로직에 의해 많은 정보가 묻혀버리기 쉽기 때문에 여러 문제를 유발한다.
* 가장 중요한 리팩토링은 Extract Method인데 코드 덩어리를 별도의 메서드로 뽑아내는 것이다. 
  * Extract Method를 사용할 때 가장 큰 문제는 지역변수와 임시변수를 다루는 것이다.
  * 임시 변수를 제거하기 위해 Replace Temp with Query를 사용한다.
  * 하나의 임시변수가 여러 목적으로 사용도니다면 Split Temporary Variable을 사용하여 쉽게 바꿀 수 있도록 한다.
  * 임시변수가 너무 꼬여있어 제거하기가 어려울 때 Replace Method with Method Object를 사용한다.
* Inline Method는 Extract Method의 반대 개념으로 메서드 호출 부분을 해당 메서드의 몸체로 바꾸는 것이다.

## 6.1 Extract Method

그룹으로 함께 묶을 수 있는 코드 조각이 있으면, 코드의 목적이 잘 드러나도록 메서드의 이름을 지어 별도의 메서드로 뽑아낸다.

### 동기

* 메서드는 잘게 쪼개져 있을 때 다른 메서드에서 사용되 확률이 높아진다.
* 고수준의 메서드를 볼 때 일련의 주석을 읽는 것 같은 느낌이 들 수 있다.
* 메서드가 잘게 쪼개져 있을 때 오버라이드 하는것도 훨씬 쉽다.
* 작은 메서드는 이름을 잘 지었을 때 진가가 드러나므로 이름을 잘 지어야한다.
* 중요한것은 메서드의 이름과 메서드의 몸체 의미적 차이이다.

### 절차

* 메서드를 새로 만들고, 의도를 잘 나타낼 수 있도록 이름을 정한다.
  * 이름을 정할 때는 메서드가 어떻게 동작하는지 이름을 짓는것 보다 무엇을 하는지 적는다.
* 원래 메서드에서 뽑아내고자 하는 부분의 코드를 복사하여 새 메서드로 옮긴다.
* 원래 메서드에서 사용되고 있는 지역변수가 뽑아낸 코드에 있는지 확인한다. 이런 지역변수는 새로운 메서드의 지역변수나 파라미터가 된다.
* 뽑아낸 코드 내에서만 사용되는 임시변수가 있는지 본다. 있다면 새로 만든 메서드의 임시변수로 선언한다.
* 뽑아낸 코드 내에서 지역변수의 값이 수정되는지 본다.
  * 만약 하나의 지역변수만 수정된다면, 뽑아낸 코드를 질의로 보고 수정된 결과를 관련된 변수에 대입할 수 있는지 본다.
  * 이 방법이 이상하거나, 값이 수정되는 지역변수가 두개 이상이라면 메서드로 쉽게 추출할 수 없다.
  * Split Temporary Variable을 사용한 다음 다시 시도한다.
  * 임시변수는 Replace Temp with Query로 제거할 수 있다.

* 뽑아낸 코드에서 읽기만 하는 변수는 새 메서드의 파라미터로 넘긴다.
* 원래 메서드에서 뽑아낸 코드 부분은 새로 만든 메서드를 호출하도록 바꾼다.

### 예제

1. 지역변수가 없는 경우

   ```java
   // 리팩토링 전
   void printOwing() {
     Enumeration e = _orders.elemnts();
     double outstanding = 0.0;
     
     // 배너 표시
     System.out.println("**************************");
     System.out.println("***** Customer Owes ******");
     System.out.println("**************************");
     
     // outstanding 계산
     while(e.hasMoreElemnts()) {
       Order each = (Order)e.nextElemnt();
       outstanding ++ each.getAmount();
     }
     
     // 상세 정보 표시
   	System.out.println("name :" + _name);
     System.out.println("amount : " + outstanding);
   }
   
   // 리팩토링 후
   void printOwing() {
     Enumeration e = _orders.elemnts();
     double outstanding = 0.0;
     
     printBanner();
     
     // outstanding 계산
     while(e.hasMoreElemnts()) {
       Order each = (Order)e.nextElemnt();
       outstanding ++ each.getAmount();
     }
     
     // 상세 정보 표시
   	System.out.println("name :" + _name);
     System.out.println("amount : " + outstanding);
   }
   
   void printBanner() {
     System.out.println("**************************");
     System.out.println("***** Customer Owes ******");
     System.out.println("**************************");
   }
   ```

   * 배너를 인쇄하는 부분을 잘라서 새 메서드로 붙여놓고 원래 코드에는 새 메서드를 호출하도록 한다.

2. 지역변수가 포함되어 있는 경우

   ```java
   void printOwing() {
     Enumeration e = _orders.elemnts();
     double outstanding = 0.0;
     
     printBanner();
     
     // outstanding 계산
     while(e.hasMoreElemnts()) {
       Order each = (Order)e.nextElemnt();
       outstanding ++ each.getAmount();
     }
     
     printDetails(outstanding)
   }
   
   void printDetails(double outstanding) {
     System.out.println("name :" + _name);
     System.out.println("amount : " + outstanding);
   }
   ```

   * 변수가 읽히기만 하고 값이 변하지 않는 경우는 쉽다.

3. 지역변수에 다른 값을 여러 번 대입하는 경우

   ```java
   void printOwing() {
     printBanner();
     double outstanding = getOutstanding();
     printDetails(outstanding)
   }
   
   double getOutstanding() {
     Enumeration e = _orders.elemnts();
     double outstanding = 0.0;
     while(e.hasMoreElements()) {
       Order each = (Order)e.nextElement();
       outstanding += each.getAmount();
     }
     return outstanding;
   }
   ```

   * 변수 e는 뽑아낸 코드 안에서만 사용되기 때문에 새 메서드로 옮길 수 있다. 변수 outstanding은 양쪽에서 모두 사용되므로 뽑아낸 메서드에서 그 값을 리턴하도록 한다.

   ```java
   // 리팩토링 전
   void printOwing(double previousAmount) {
     Enumeration e = _orders.elements();
     double outstanding = previousAmount * 1.2;
     
     printBanner();
     
     while (e.hasMoreElements()) {
       Order each = (Order)e.nextElemnt();
       outstanding += each.getAmount();
     }
     
     printDetails(outstanding);
   }
   
   // 리팩토링 후 1
   void printOwing(double previousAmount) {
     double outstanding = previousAmount * 1.2;
     printBanner();
     double outstanding = getOustanding(outstanding);
     printDetails(outstanding);
   }
   
   double getOutstanding(double initialValue) {
     double result = initialValue;
     Enumeration e = _orders.elements();
     while(e.hasMoreElements()) {
       Order each = (Order)e.nextElement();
       result += each.getAmount();
     }
     return result;
   }
   
   // 리팩토링 후 2
   void printOwing(double previousAmount) {
     printBanner();
     double outstanding = getOustanding(previousAmount * 1,2);
     printDetails(outstanding);
   }
   ```

   * 처음의 경우 outstanding이 명확한 초기값으로 초기화하기 때문에 새로 뽑아낸 메서드안에서 초기화 할 수 있었다.
   * 그렇지 않다면 파라미터로 값을 넘겨야한다.

* 만약 두개 이상의 값이 리턴되어야 한다면, 뽑아낼 코드를 다르게 선택하여 두개의 새로운 메서드를 만들거나, 출력 파라미터를 지원하면 출력 파라미터를 이용할 수 있다.

* 임시 변수가 너무 많아 코드를 뽑아내기 어려운 경우가 있다. 이런 경우에는 Replace Temp with Query를 사용하여 임시변수를 줄인다.
* 여전히 난처한 경우라면 Replace Method with Metho Object를 사용한다.

## 6.2 Inline Method

메서드 몸체가 메서드의 이름 만큼이나 명확할 때는 호출하는 곳에 메서드의 몸체를 넣고, 메서드를 삭제하라.

### 동기

* 리팩토링하다보면 메서드의 몸체가 메서드의 이름 만큼이나 명확할 때가 있다. 또는 메서드의 몸체를 메서드의 이름만큼 명확하게 리팩토링 할 수도 있다.
* Inline Method는 메서드가 잘못 나누어져 있을 때도 사용할 수 있다. 나누어져 있는 메서드를 다시 하나로 합쳐 하나의 큰 메서드를 만든다음, 메서드를 다시 추출한다.
* 메서드 객체가 포함하고 있어야 할 동작을 가진 메서드에 의해 호출되는 여러 메서드를 인라인화 한다.
* 너무 많은 인디렉션이 사용되어 모든 메서드가 단순히 다른 메서드에 위임을 하고 있을 때도 사용한다.

### 절차

* 메서드가 다형성을 가지고 있는지 확인한다.
  * 서브클래스에서 오버라이드 하고 있는 메서드에는 적용하지 않는다.
* 메서드를 호추하고 있는 부분을 모두 찾는다.
* 각각의 메서드 호출을 메서드 몸체로 바꾼다.
* 절차만 봐서는 간단해보이지만, 재귀가 사용되거나 리턴 포인트가 여러곳인 경우에는 복잡하다. 이런 경우는 Inline Method 사용하지 않는것이 좋다.

### 예시

```java
// 리팩토링 전
int getRating() {
  return (moreThanFivelateDeliveries()) ? 2 : 1;
}
boolean moreThanFiveDeliveries() {
  return _numberOfLateDeliveries > 5;
}

// 리팩토링 후
int getRating() {
  return (_numberOfLateDeliveries > 5) ? 2 : 1;
}
```



## 6.3 Replace Temp with Query

어떤 수식의 결과값을 저장하기 위해서 임시변수를 사용하고 있다면, 수식을 뽑아내서 메서드로 만들고, 임시변수를 참조하는 곳을 찾아 모두 메서드 호출로 바꾼다. 새로 만든 메서드는 다른 메서드에서도 사용할 수 있다.

### 동기

* 임시변수는 임시로 사용되고, 특정 부분에서만 의미를 가지므로 문제가 된다.
* 임시변수가 사용되는 경우는 보통 길이가 길어지는 경향이 잇다. 임시변수에 접근할 유일한 방법이기 때문이다.
  * 임시변수를 질의 메서드로 바꿈으로써 클래스 내의 어떤 메서드도 임시변수에 사용될 정보를 얻을 수 있다.
* Replace Temp with Query는 Extract Method를 적용하기 전의 필수 단계이다. 지역변수는 메서드의 추출을 어렵게 하기 때문에 가능한 많은 지역변수를 질의 메서드로 바꾸는 것이 좋다.
* 이 리팩토링을 적용하는 데 있어 가장 간단한 경우는 임시변수에 값이 한번만 대입되고, 대입문을 만드는 수식이 부작용을 초래하지 않는 경우이다.
  * 다른 경우는 Split Temporary Variable이나 Seperate Query From Modifier를 먼저 적용하는 것이 쉬울 것이다.

### 절차

간단한 경우에 대한 절차는 다음과 같다.

1. 임시변수에 값이 한번만 대입 되는지 확인한다.
   * 임시변수에 값이 여러번 대입되는 경우는 Split Temporary Variable을 먼저 적용한다.
2. 임시변수를 final로 선언한다.
3. 대입문의 우변을 메서드로 추출한다.
   * 처음에는 메서드를 private으로 선언한다. 나중에 다른 클래스에서도 사용하는것이 좋을것 같으면 접근자를 바꾼다.
   * 추출된 메서드에 부작용이 없는지 확인한다. (객체의속성을 바꾸는 등..) 부갖용이 있다면 Seperate Query from Modifier를 사용한다.

* 임시변수는 종종 루프에서의 요약정보를 저장하는 데 사용된다. 루프 전체가 하나의 메서드로 추출될 수 있는데 이렇게 하면 잡다한 코드를 몇 줄 없앨 수 있다.
* 때로는 루프 안에서 여러 변수에 대한 합을 구할 수도 있다. 이런 경우에는 각각의 임시변수에 대해 루프를 중복시켜 각 임시변수를 질의로 바꿀 수 있도록 한다.
* 퍼포먼스의 문제가 있을 경우가 있는데 최작화 단계에서 수정한다.

### 예제

```java
double getPrice() {
  int basePrice = _quantity * _itempPrice;
  double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}

// 리팩토링 1
double getPrice() {
  final int basePrice = _quantity * _itempPrice;
  final double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}

// 리팩토링 2
double getPrice() {
  final int basePrice = basePrice();
  final double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}
private int basePrice() {
  return _quantity * _itempPrice;
}

// 리팩토링 3
double getPrice() {
  final double discountFactor;
  if (basePrice() > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice() * discountFactor;
}

// 다시 리팩토링 2
double getPrice() {
  final double discountFactor = discountFactor();
  return basePrice() * discountFactor;
}
private double discountFator() {
  if(basePrice() > 1000) return 0.95;
  else return 0.98;
}

// 다시 리팩토링 3
double getPrice() {
  return basePrice() * discountFactor();
}
```

* 임시변수를 먼저 final로 선언하여 컴파일한다. 컴파일을 하면 어떤 문제가 있을 경우 바로 알 수 있다. 그러면 이 리팩토링을 사용하지 않으면 된다.
* 임시변수의 우변을 메서드로 추출한 뒤 임시변수를 참조하는 곳도 바꾼다.



## 6.4 Introduce Explainig Variable

복잡한 수식이 있는 경우에는 수식의 결과나 수식의 일부에 자신의 목적을 잘 설명하는 이름으로 된 임시변수를 사용하라.

### 동기

* 수식은 매우 복잡해져 알아보기가 어려울 수 있다. 이런 경우는 임시변수가 수식을 좀 더 다루기 쉽게 나누는데 도움이 될 수 있다.
  * Introduce Explaining Variable은 특히 조건문에서 각각의 조건의 뜻을 잘 설명하는 이름의 변수로 만들어 사용할 때 유용하다.
* Introduce Explaining Variable보다 Extract Method가 다른 객체에서도 사용할 수 있기 때문에 더 유용하다.
  * 그러나 지역변수 때문에 Extract Method가 사용하기 어려울 때도 있다 그럴때 Introduce Explaining Variable을 사용한다.

### 절차

1. final 변수를 선언하고, 복잡한 수식의 일부를 이 변수에 대입한다.
2. 원래의 복잡한 수식에서 임시변수에 대입한 수식을 임시변수로 바꾼다.
3. 컴파일 테스트를 한다.

### 예제

```java
// 리팩토링 전
double price() {
  // price = (base price) - (quantity discount) + (shipping)
  return _quantity * _itemPrice -
    Math.max(0, _quantity - 500) * _itemPrice * 0.05 +
    Math.min(_quantity * _itemPrice * 0.1, 100);
}

// 리팩토링 1
double price() {
  // price = (base price) - (quantity discount) + (shipping)
  final double basePrice = _quantity * _itemPrice;
  return basePrice -
    Math.max(0, _quantity - 500) * _itemPrice * 0.05 +
    Math.min(basePrice * 0.1, 100);
}

// 리팩토링 2
double price() {
  // price = (base price) - (quantity discount) + (shipping)
  final double basePrice = _quantity * _itemPrice;
  final double quantityDiscount = Math.max(0, _quantity - 500) * _itemPrice * 0.05;
  return basePrice - quantityDiscount + 
    Math.min(basePrice * 0.1, 100);
}

// 리팩토링 3
double price() {
  final double basePrice = _quantity * _itemPrice;
  final double quantityDiscount = Math.max(0, _quantity - 500) * _itemPrice * 0.05;
  final double shipping = Math.min(basePrice * 0.1, 100);
  return basePrice - quantityDiscount + shipping;
}

// 리팩토링 4 (Extract Method)
double price() {
  return basePrice() - quantityDiscount() + shipping();
}
private double basePrice() {
  return _quantity * _itemPrice;
}
private double quantityDiscount() {
  return Math.max(0, _quantity - 500) * _itemPrice * 0.05;
}
private double shipping() {
  return Math.min(basePrice * 0.1, 100);
}
```

* 각 수식에 대해 임시변수를 넣어 표현한다. 그러면 주석이 필요하지 않다.

* Extract Method로도 리팩토링 할 수있다. Extract Method를 이용하면 객체의 어느 부분에서나 메서드 호출로 사용될 수 있다.
  * 수많은 지역변수를 사용하고 있는 알고리즘의 경우는 Extract Method를 사용하는것이 어렵다. 이럴 경우 Introduce Explaining Variable을 사용한다.

## 6.5 Split Temporary Variable

루프 안에 있는 변수나 collecting temporary variable도 아닌 임시변수에 값을 여러번 대입하는 경우에는 각각의 대입에 대해 따로따로 임시변수를 만들어라.

### 동기

* for문의 collecting temporary variable(i++같은거)가 아닌 임시변수는 주로 긴 코드에서 계산한 결과값을 나중에 쉽게 참조하기 위해 보관하는 용도로 사용한다.
* 어떤 변수든 여러 가지 용도로 사용되는 경우에는 각각의 용도에 대해 따로 변수를 사용해야한다.

### 절차

1. 임시변수가 처음 선언된 곳과 처음 대입된 곳에서 변수의 이름을 바꾼다.
2. 새로 만든 임시변수를 final로 선언한다.
3. 임시변수에 두 번째로 대입하는 곳의 직전까지 원래 임시변수를 참조하는 곳을 모두 바꾼다.
4. 임시변수에 두 번째로 대입하는 곳에서 변수를 선언한다.
5. 컴파일과 테스트를 한다.

### 예제

```java
// 리팩토링 전
double getDistanceTravelled(int time) {
  double result;
  double acc = _primaryForcce / _mass;
  int primaryTime = Math.min(time, _delay);
  result = 0.5 * acc * primaryTime * primaryTime;
  int secondaryTime = time - _delay;
  if (secondaryTime > 0) {
    double primaryVel = acc * _delay;
    acc = (_primaryForce * _secondaryForce) / _mass;
    result += primaryVel * secondaryTime + 0.5 * acc * secondaryTime * secondaryTime;
  }
  return result;
}

// 리팩토링 후
double getDistanceTravelled(int time) {
  double result;
  final double primaryAcc = _primaryForcce / _mass;
  int primaryTime = Math.min(time, _delay);
  result = 0.5 * primaryAcc * primaryTime * primaryTime;
  int secondaryTime = time - _delay;
  if (secondaryTime > 0) {
    double primaryVel = acc * _delay;
    final double secondaryAcc = (_primaryForce * _secondaryForce) / _mass;
    result += primaryVel * secondaryTime + 0.5 * secondaryAcc * secondaryTime * secondaryTime;
  }
  return result;
}
```

* 변수 acc에 값이 두 번 설정된다. 즉 두가지 용도로 사용된다.
* 임시변수의 이름을 바꾸고 final로 선언한다.
* 추가적으로 다양한 리팩토링을 적용할 수 있다.

## 6.6 Remove Assignments to Parameters

파라미터에 값을 대입하는 코드가 있으면 임시변수를 대신 사용해라.

### 동기

* 파라미터에 값을 대입한다는 것은 다른 객체를 참조하게 하는것이다. 객체내의 메서드를 호출하는 것은 아무런 문제가 없다.
  * `object.getValue()`는 문제가 없지만, `object = anotherObject`는 문제가 있다.
* 파라미터에 값을 대입해서는 안되고, Remove Assignment to Parameters를 적용해야한다.

### 절차

1. 파라미터를 위한 임시변수를 만든다.
2. 파라미터에 값을 대입한 코드 이후에서 파라미터에 대한 참조를 임시변수로 바꾼다.
3. 파라미터에 대입하는 값을 임시변수에 대입하도록 바꾼다.
4. 컴파일과 테스트를 한다.

### 예제

```java
// 리팩토링 전
int discount(int inputVal, int quantity, int uearToDate) {
  if (inputVal > 50) inputVal -= 2;
  if (quntity > 100) inputVal -= 1;
  if (yearToDate > 10000) inputVal -= 4;
  return inputVal;
}

// 리책토링 후
int discount(int inputVal, int quantity, int uearToDate) {
	int result = inputVal;
  if (inputVal > 50) result -= 2;
  if (quntity > 100) result -= 1;
  if (yearToDate > 10000) result -= 4;
  return result;
}
```



## 6.7 Replace Method with Method Object

긴 메서드가 있는데, 지역변수 때문에 Extract method를 적용할 수 없는 경우에는 메서드를 그 자신을 위한 객체로 바꿔서 모든 지역변수가 그 객체의 필드가 되도록한다. 이렇게 하면 메서드를 같은 객체 안의 여러 메서드로 분해할 수 있다.

### 동기

* 지역변수가 많아지면 분해가 어려워질 수 있다. Replace Temp with Query는 이런 짐을 덜도록 도와주지만 때로는 조개야 하는 메서드를 쪼갤 수 없는 경우가 생길 수도 있다.
  * 이런 경우에는 메서드 객체를 꺼내 사용한다.
* Replace Method with Method Object는 지역변수를 메서드 객체의 필드로 바꿔버린다. 그 후 새로운 객체에 Extract Method를 사용하여 원래의 메서드를 분해한다.

### 절차

1. 메서드의 이름을 따서 새로운 클래스를 만든다.
2. 새로운 클래스에 원래 메서드가 있던 객체를 보관하기 위한 final 필드를 하나 만들고, 메서드에서 사용되는 임시변수와 파라미터를 위한 필드를 만든다.
3. 새로운 클래스에 소스 객체와 파라미터를 취하는 생성자를 만든다.
4. 새로운 클래스에 compute라는 이름의 메서드를 만든다.
5. 원래의 메서드를 compute 메서드로 복사한다. 원래의 객체에 있는 메서드를 사용하는 경우에는 소스 객체 필드를 사용하도록 바꾼다.
6. 컴파일한다.
7. 새로운 클래스의 객체를 만들고 원래의 메서드를 새로 만든 객체의 compute 메서드를 호출하도록 한다.

### 예제

```java
// 리팩토링 전
class Account {
  ...
  int gamma(int inputVal, int quantity, int yearToDate) {
    int importantValue1 = (inputVal * quantity) + delta();
    int importantValue2 = (inputVal * yearToDate) + 100;
    if((yearToDate - importantValue1) > 100) {
      importantValue2 -= 20;
    }
    int importantValue3 = importantValue2 * 7;
    ...
    return importantValue3 - 2 * importantValue1;
  }
}

// 리팩토링 후
class Gamma {
  private final Account _account;
  private int inputVal;
  private int quantity;
  private int yearTodate;
  private int importantVal1;
	private int importantVa2;
	private int importantVa3;
  
  Gamma(Account source, int inputValArg, int quantityArg, int yearToDateArg) {
    _account = source;
    inputVal = inputValArg;
    quantity = quantityArg;
    yearToDate = yearToDateArg;
  }
  
  int compute() {
    int importantValue1 = (inputVal * quantity) + delta();
    int importantValue2 = (inputVal * yearToDate) + 100;
    if((yearToDate - importantValue1) > 100) {
      importantValue2 -= 20;
    }
    int importantValue3 = importantValue2 * 7;
    ...
    return importantValue3 - 2 * importantValue1;
  }
}
class Account {
  ...
  int gamma(int inputVal, int quantity, int yearToDate) {
    return new Gamma(this, inputVal, yearToDate).compute();
  }
}
```

* gamma()메서드를 메서드 객체로 바꾸기 위해서, 새로운 클래스를 만든다. 원래의 객체를 위한 final 필드와 메서드의 파라미터와 임시변수를 위한 필드를 넣는다.

* 생성자를 추가한다.
* 원래 메서드에 있는 메서드를 compute()메서드로 옮긴다.
* 이전의 메서드가 메서드 객체로 위임하도록 수정한다.
* 이제 compute 메서드에서 파라미터를 넘겨주는것에 대한 걱정 없이 쉽게 Extract Method를 사용할 수 있다.

## 6.8 Substitute Algorithm

알고리즘을 보다 명확한 것으로 바꾸고 싶을 때는, 메서드의 몸체를 새로운 알고리즘으로 바꾼다.

### 동기

* 어떤 것을 할 대 더 명확한 방법을 찾게 되면, 복잡한 것을 명확한 것으로 바꿔야한다.
* 어떤 때는 일을 조금 다르게 처리하기 위해 알고리즘을 바꾸고 싶을 때가 있는데, 원하는 변경을 하기 위해 먼저 간단한 것으로 치환하는 것이 더 쉽다.
* 이 단계를 거쳐야 할 때 가능한 많이 메서드를 분해해 두어야 한다.

### 절차

* 대체 알고리즘을 준비한다. 적용하여 컴파일한다.
* 테스트한다. 같은 결과가 나오지 않는다면 원복하여 디버깅한다.



