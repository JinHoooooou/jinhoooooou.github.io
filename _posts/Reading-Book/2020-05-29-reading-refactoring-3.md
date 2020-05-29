---
title: "[Reading Book]Refactoring(3)"
excerpt: "3장 코드 속의 나쁜 냄새"
classes: wide
categories:
 - Book
tags:
 - clean code
 - refactoring
last_modified_at: 2020-05-29
---



# 3장 코드 속의 나쁜 냄새

* 리팩토링을 언제 시작하고, 언제 끝낼지를 결정하는 것은 리팩토링을 어떤 절차에 따라 해야하는지를 아는 것 만큼 중요하다.
* 얼마나 많은 인스턴스 변수가 지나친 것인지, 어느 정도의 길이가 지나치게 긴 메서드인지 자신만의 감각을 개발해야한다.

## 3.1 중복된 코드(Duplicated Code)

* 한 클래스의 서로 다른 두 메서드 안에 같은 코드가 있는 경우
  * Extract Method후, 뽑아낸 메서드를 호출한다.
* 동일한 super 클래스를 갖는 두 sub 클래스에서 같은 코드가 있는 경우
  * Extract Method 후 pull UP method를 사용한다.
* 코드가 비슷하지만 같지는 않은 경우
  * 같은 부분과 다른 부분을 분리하기 위해 Extract Method 사용 후 Form Template Method를 사용할 수 있는지 알아본다.
* 같은 작업을 하지만 다른 알고리즘을 사용하는 경우
  * 더 명확한 알고리즘을 선택한 후, Substitute Algorithm을 사용한다.
* 서로 관계가 없는 두 클래스에서 중복된 코드가 있는 경우
  * 한쪽 클래스에서 Extract Class를 사용 한 뒤 양쪽에서 새로운 클래스를 사용하도록 고려한다.

## 3.2 긴 메서드 (Long Method)

* 메서드의 길이는 짧아야한다.
* 짧은 메서드로 된 코드를 쉽게 이해하려면 이름을 잘 지어야한다.
  * 코드의 의도를 잘 나타내는 이름을 지어야한다.
* 대부분의 경우 메서드 길이를 줄이기 위해 Extract Method를 사용한다.
* 메서드에 파라미터와 임시변수가 많은 경우
  * 임시 변수 제거를 위해 Replace Temp with Query를 사용한다.
  * 긴 파라미터 리스트는 Introduce parameter Object와 Preserve Whole Object로 짧게 할 수 있따.
  * 여전히 임시변수와 파라미터가 많다면 Replace Method with Method Object를 사용한다.
* 조건문과 루프 또한 메서드 추출이 필요하다.
  * 조건식을 다루기 위해 Decompose Conditional을 사용한다.
  * 루프의 경우는 루프와 그 안의 코드를 Extract Method를 사용한다.

## 4.3 거대한 클래스 (Large Class)

* 클래스 하나가 너무 많은 일을 하려 할 때는 보통 지나치게 많은 인스턴스 변수가 나타난다.
  * 많은 변수를 묶기 위해 Extract Class를 사용한다.

+ 클래스가 모든 인스턴스 변수를 항상 다 사용하지 않는경우

  + Extract Class와 Extract Subclass를 여러번 적용한다.

+ 지나치게 많은 코드를 가진 클래스

  + 메서드와 마찬가지로 클래스 자체 내에서 중복을 제거한다.

  * Extract Method나 Extract Subclass를 사용한다.

+ 큰 클래스가 GUI클래스라면 데이터와 동작을 별도의 도메인 객체로 옮길 필요가 있다.

  + 이것은 양쪽에 약간의 중복된 데이터를 유지하게 하고, 데이터를 동기화 해주는 것이 필요하다 
  + Duplicate Observed Data가 이 작업을 어떻게 할 것인지 보여준다.
  + 🙋‍♂️ 이 방법을 봐야지 이해를 할 수 있을것 같다. 아직 무슨말인지 명확히 이해하지 못함..

## 4.4 긴 파라미터 리스트 (Long Parameter List)

* 객체 지향 언어에서 한 객체에 필요한 무언가가 없을 때 다른 객체에게 물어서 얻을 수 있다. 따라서 객체를 사용한다면, 모든 것을 파라미터로 넘길 필요 없다.
* 긴 파라미터 리스트는 이해하기도 어렵고, 일관성이 없거나 사용하기 어려울 뿐 아니라 다른 데이터가 필요할 때마다 계속 고쳐야하기 때문에, 파라미터 리스트는 짧은 것이 좋다.
* 이미 알고 있는 객체에 요청하여 파라미터의 데이터를 얻을 수 있을 때
  * Replace Parameter whit Method를 사용한다.
  * 한 객체로부터 주워 모은 데이터 뭉치를 그 객체 자체로 바꾸기 위해 Preserve Whole Object를 사용한다.
  * 객체와 관계 없는 여러 개의 아이템이 있으면 Introduce Parameter Object를 사용한다.
* 호출하는 객체와 큰 객체 사이의 종속성을 만들고 싶지 않을 때
  * 데이터를 하나씩 풀어서 파라미터로 넘긴다.

## 4.5 확산적 변경 (Divergent Change)

* 소프트웨어는 유연해야한다. 따라서 변경하기 쉬운 구조로 만들어야한다.
* 소프트웨어를 변경할 때 명확한 곳을 집어 변경할 수 있어야한다.
* 확산적 변경은 한 클래스가 다른 방법으로 자주 변경되는 경우에 발생한다.
  * 새로운 데이터베이스를 추가할 때마다 어느 메서드를 수정해야 하는경우
  * 새로운 어음이 있을 때마다 항상 메서드를 변경해야 하는경우
  * 특정 원인에 대해 변해야 하는 것을 모두 찾은 후 Extract Class를 사용하여 하나로 묶는다.
  * 🙋‍♂️ 이것도 예시를 봐야 명확히 이해가 될것같다. 

## 4.6 산탄총 수술 (Shotgun Surgery)

* 산탄총 수술은 확산적 변경과 반대이다.
* 변경 할 때마다 많은 클래스를 조금씩 수정해야 한다면 산탄총 수술의 냄새를 풍기고 있는 것이다.
* 확산적 변경은 여러 종류의 변경 때문에 하나의 클래스가 시달리는 경우이고, 산탄총 수술은 하나를 변경했을 때 많은 클래스를 고쳐야 하는 경우이다.
  * Move Metod, Move Field를 사용하여 변경해야 할 부분을 하나의 클래스로 묶는다.
  * 종종 Inline Class를 사용하여 모든 동작을 하나로 모울 수도 있다.

## 4.6 기능에 대한 욕심 (Feature Envy)

* 메서드가 자신이 속한 클래스보다 다른 클래스에 관심을 가지고 있는 경우
  * 다른 객체의 get()메서드를 사용하는 호출하는 예
  * 그 메서드는 다른 곳에 있고 싶은 것이기 때문에 Move Method를 사용한다.
  * 또는 Extract Method를 사용한 후 Move Method를 사용한다.
* 한 메서드가 여러 개의 클래스의 기능을 사용하는 경우
  * 어떤 클래스에 있는 데이터를 가장 많이 사용하는 지 보고 그 클래스로 Move Method를 사용한다.
* 이런 규칙이 깨지는 몇몇 복잡한 ㅊ패턴도 있다.
  * 디자인 패턴에서 Strategy와 Visitor가 대표적이다.
  * 🙋‍♂️ 이 부분은 이해가 잘 안된다 ㅜ

## 4.7 데이터 덩어리 (Data Clump)

* 2~3개의 클래스에 있는 필드, 메서드의 파라미터 등과 같이 데이터 아이템들이 모여 있는것을 볼 수 있다.
* 함께 몰려다니는 데이터의 무리는 객체로 만들어져야 한다.
  * 필드로 나타나는 덩어리들이 있는 곳을 찾는다. 이 덩어리를 객체로 바꾸기 위해 필드들에 대해 Extract Class를 사용한다.
  * 메서드에서 Introduce Parameter Object나 Preserve Whole Object를 사용하여 파라미터 리스트를 단순하게 한다

## 4.8 기본 타입에 대한 강박관념 (Primitive Obsession)

* 객체의 유용한 점 중 하나는 기본 타입과 레코드 타입의 경계를 흐리게 하거나 없앤다.
* 각각의 데이터 값에 대해 Replace Data Value with Object를 사용하여 객체로 만들 수 있다.
* 데이터 값이 타입 코드이고, 값이 동작에 영ㅎ야을 미치지 않는다면 Replace Type Code with Class를 사용한다.
* 타입 코드에 의존하는 조건문이 있는 경우에는 Replace Type Code with Subclass 또는 Replace Type Code with State/Strategy를 사용한다.
* 항상 몰려다녀야 할 필드 그룹이 있다면 Extract Class를 사용한다.
* 파라미터 리스트에서 기본 타입을 보면 Introduce parameter object를 사용한다.
* 배열을 쪼개서 쓰고 있다면 Replace Array with Object를 사용하라.

## 4.9 Switch 문 (Switch Statements)

* 객체지향 코드의 가장 명확한 특징 중 하나는 switch문이 비교적 적게 쓰인다는 것이다.
* switch문은 본질적으로 중복된다. 다형성을 이용하여 이런 문제를 다룰 수 있다.
* switch문은 보통 타입 코드를 사용한다. 메서드나 클래스가 타입 코드 값을 갖고 있기를 원할 것이다.
  * 따라서 Extract Method를 사용하여 switch문을 뽑아내고 Move Method를 사용하여 다형성이 필요한 클래스로 옮긴다.
  * Repalce Type  Code with Subclasses를 사용할지, Replace Type Code with State/Strategy를 사용할지 결정해야한다.
  * 상속 구조를 결정했으면, Replace Conditional with Polymorphism을 사용할 수 있다.
* 만약 하나의 메서그에만 영향을 미치는 몇 개의 경우가 있다면 굳이 바꿀 필요 없다.
  * Replace Parameter with Explicit Methods가 좋은 선택이다.
* 조건중 null이 있는 경우가 있다면 Introduce Null Object를 사용해라

## 4.10 평행 상속 구조(Parallel Inheritance Hierarhies)

* 평행 상속 구조는 산탄총 수술의 특별한 경우이다. 이런 경우 한 클래스의 서브클래스를 만들면 다른 곳에서도 서브클래스를 만들어 주어야 한다.
* 중복을 제거하는 일반적인 전략은 한쪽 상속 구조의 인스턴스가 다른 쪽 구조의 인스턴스를 참조하도록 만드는 것이다.
  * Move Method와 Move Field를 사용한다.

## 4.11 게으른 클래스(Lazy Class)

* 클래스를 생성할 때마다 유지하고 이해할 비용이 발생한다. 이 비용을 감당할 만큼 충분한 일을 하지 않는 클래스는 삭제한다.
  * 하는 일이 별로 없는 클래스의 서브클래스가 있다면, Collapse Hierarchy를 사용한다.
  * 거의 필요 없는 클래스에 대해서는 Inline Class를 사용한다.

## 4.12 추측성 일반화 (Speculative Generality)

* "언젠가 이 기능이 필요할 것 같은데.."라며 필요하지 않은 것을 처리 하기 위해 코드가 늘어나면 코드는 더 이해하기 어려워지고 유지보수가 어려워진다.
* 하는 일이 별로 없는 추상 클래스가 있다면, Collapse Hierarchy를 사용한다.
* 부릴요한 위임은 Inline Class로 제거할 수 있다.
* 메서드에 사용되지 않는 파라미터가 있다면 Remove Parameter를 적용해야한다.
* 메서드 이름이 이상하고 추상적일 때는 Rename Parameter를 적용한다.
* 추측성 일반화는 어떤 메서드나 클래스가 테스트 케이스에서만 사용되는 경우에도 발견될 수 있다. 해당 메서드 뿐만 아니라 테스트 케이스도 삭제하라.

## 4.13 임시 필드 (Temporary Field)

+ 객체 안의 인스턴스 변수가 특정 상황에서만 세팅되는 경우가 있다. 사용되지 않는 것처럼 보이는 변수가 왜 있는지를 이해하는것은 매우 짜증난다.
  + 고아 변수들을 위한 집을 만들기 위해 Extract Class를 사용한다. 그리고 그 변수를 사용하는 모든 코드를 새로 만든 클래스에 넣는다.
  + 유효하지 않는 변수에 대한 대체 컴포넌트를 만들기 위해 Introduce Null Object를 사용한다.
+ 임시 필드는 복자반 알고리즘이 여러 변수를 필요로 할 때 필요로 할 때 나타난다. 이런 필드는 그 알고리즘에 대해서만 유효할 뿐 다른 데에서는 혼란만 초래한다.
  + 필요한 변수와 메서드를 묶어 Extract Class를 사용한다.

## 4.14 메시지 체인 (Message Chains)

+ get()메서드를 계속 호출하는 경우 메세지 체인을 볼 수 있다. 이런식은 클래스 구조와 클라이언트가 결합되어 있다는 것을 말한다.
  + Hide Delegate를 사용한다. 체인 내의 모든 객체에 적용할 수도 있지만 종종 중간에 있는 객체를 미들 맨으로 만드는 결과를 초래한다.
  + 결과적으로 어던 객체가 사용되는지를 보는것이 나을 수도 있다. 사용하는 코드의 조각을 취해 Extract Method를 사용할 수 있는지 보고, Move Method로 체인의 밑으로 밀어넣는다.
  + 🙋‍♂️ 메시지 체인이 뭘 나타내는지는 알겠는데 해결책에 대해서는 무슨말인지 이해를 못하겠다 ㅜ

## 4.15 미들 맨 (Middle Man)

+ 객체의 주요 특징 중 하나가 캡슐화이다. 그러나 캡슐화가 너무 지나칠 때가 있다.
+ 클래스의 인터페이스를 보니 메서드의 태반이 다른 클래스로 위임을 하고 있다면 Remove Middle Man을 사용하여 그 객체에 실제로 뭐가 어떻게 되어가고 있는지를 알게 해줘야한다.
+ 몇몇 메서드가 많은 일을 하지 않는다면 Inline method를 사용하여 호출하는 곳에 코드를 삽입할 수 있다.
+ 추가동작이 있다면 Replace Delegation with Inheritance를 사용하여 미들맨을 실제 객체의 서브클래스로 바꿀 수도 있다.

## 4.16 부적절한 친밀 (Inappropriate Intimacy)

* 클래스가 지나치게 친밀하게 되어 서로 사적인 부분을 파고드느라 많은 시간을 소모할 수 있다.
  * Move Method와 Move Field를 사용하여 조각으로 나누고 친밀함을 줄여야한다.
  * Chnage Bidirectional Association to Unidirecctional이 적용 가능한지 봐라.
  * 클래스에 공통 관심사가 있다면 Extract Class를 사용하여 공통된 부분을 안전한 곳으로 빼내서 별도의 클래스를 만들어라. 또는 Hide Delegate를 사용하여 다른 클래스가 중개하도록 하라.
* 상속은 종종 과도한 친밀을 유도할 수 있다. 서브클래스는 항상 그 부모클래스가 알려주고 싶은 것보다 많은 것을 알려고한다. 분리할 때 Replace Inheritance with Delegation을 적용하라.

## 4.17 다른 인터페이스를 가진 대체 클래스 (Alternative Classes with Different Interface)

* 같은 작업을 하지만 다른 시그너처를 가지는 메서드에 대해서 Rename Method를 사용하라
* 프로토콜이 같아질 때까지 Move Method를 이용하여 동작을 이동시켜라.
* 너무 많은 코드를 옮겨야 할 때는 Extract Superclass를 사용한다.

## 4.18 불완전한 라이브러리 클래스 (Incomplete Library Class)

* 라이브러리 클래스가 가지고 잇었으면 하는 메서드가 있다면 Introduce Foreign Method를 사용하라.
* 별도의 동작이 잔뜩 있다면 Introduce Local Extension이 필요하다.

## 4.19 데이터 클래스 (Data Class)

* 필드에 대한 get/set 메서드만 가지고 아무것도 없는 클래스가 있다.
  * 이런 클래스는 보통 데이터만 저장하고 대부분 다른 클래스에 의해 조작된다.
  * 초기에는 public 필드를 가진다. Encapsulate Field를 적용한다.
* 컬렉션 필드를 가지고 있다면 적절히 캡슐화 되어 있는지 확인하고, 캡슐화가 되어 있지 않다면 Encapsulate Colletion을 적용한다.
* 값이 변경되면 안되는 필드에 대해서는 Remove setting Method를 사용한다.
* get/set 메서드가 다른 클래스에서 사용되는지 찾아보고 , 동작을 데이터 클래스로 옮기기 위해 Move Method를 시도한다. 
* 메서드 전체를 옮길 수 없을 때는 Extract Metohd를 사용해서 옮길 수 있는 메서드로 만들어라.

## 4.20 거부된 유산 (Refused Bequest)

* 서브클래스는 메서드와 데이터를 부모클래스로부터 상속 받는다. 그러나 서브클래스가 그들에게 주어진 것을 원치 않거나 필요하지 않는다면 어떨까?
  * Push Down Method와 Push Down Field를 사용하여 사용되지 않는 메서드를 새로 만든 형제 클래스로 옮겨야한다.

## 4.21 주석 (Comments)

* 코드 block이 모슨 작업을 하는지 설명하기 위해 주석이 필요하다면 Extract Method를 시도한다.
* 메서드가 이미 추출되어 있는데도 여전히 코드가 하는 일에 대한 주석이 필요하다면 Rename Method를 사용해라.
* 시스템의 필요한 상태에 대한 규칙같은 것을 설명할 필요가 있다면 Introduce Assertion을 사용하라.