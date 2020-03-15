---
title: "Clean Code 읽기(5)"
excerpt: "12장 창발성(創發性)"

categories:
 - blog
tags:
 - blog
last_modified_at: 2020-03-05
---



## 12장 창발성(創發性)

* 창발적 설계로 깔끔한 코드를 구현하자

  * SRP(Single Responsibility Principle, 단일 책임 원칙) - 모든 클래스는 하나의 책임만 가지며, 그 책임을 캡슐화해야 한다.
  * DIP(Dependency Inversion Principle, 의존 관계 역전 원칙) 
    * 상위 모듈은 하위 모듈에 의존해서는 안된다. 상위 모듈 하위 모듈 모두 추상화에 의존해야한다.
    * 추상화는 세부 사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야한다.

* 단순한 설계 규칙 1 : 모든 테스트를 실행하라

  테스트가 가능한 시스템을 설계해야한다. 검증이 불가능한  시스템은 출시해서는 안된다.

  테스트 가능한 시스템 설계 --> 설계 품질이 높아질 수 밖에 없다.

  결합도가 높으면 테스트 케이스를 작성하기 어렵다. 따라서 테스트 케이스를 많이 작성할 수록 DIP같은 원칙을 적용하여 결합도를 낮추고, 설계품질은 향상된다.

* 단순한 설계 규칙 2~4 : 리팩터링

  테스트 케이스를 작성하면 코드와 클래스를 정리해도 된다. 테스트 코드가 있으므로 시스템이 무너질까 걱정할 필요 없다.

  리팩토링 단계에서는 응집도를 높히고, 결합도를 낮추고, 함수와 클래스 크기를 줄이고 설계 품질을 높이는 기법은 무엇이든 사용해도 괜찮다.

* 중복을 없애라

  공통적인 코드를 새 메소드로 추출하여 중복을 없앨 수 있다.

* 표현하라

  코드는 개발자의 의도를 분명히 표현해야한다. 그래야 결함이 줄어들고 유지보수 비용이 적게든다.

  * 좋은 이름 선택하기
  * 함수, 클래스 크기를 최대한 줄이기
  * 표준 명칭 사용 : command나 visitor같은 표준 패턴을 사용하면 설계 의도를 이해하기 쉬워진다.
  * 단위 테스트 케이스 작성 

* 클래스와 메서드 수를 최소로 줄여라

  클래스와 메서드 크기를 줄이다보면 수없이 만들어지는 경우가 있다. 

  목표는 크기를 작게 유지하되 시스템 크기도 작게 유지하는것이다.

  하지만 의도를 표현하는 작업이 더 중요하기 때문에 우선순위는 낮게 잡는다.

코드를 작성하는 시간도 많지만 코드를 읽고 이해하는 시간도 많다고 생각한다. 어느 코드를 볼 때 빠르게 이해하려면 코드에 개발자의 의도가 잘 드러나야한다고 생각한다. 그런 클린코드를 작성하기 위해서 계속 노력해야 한다!!
