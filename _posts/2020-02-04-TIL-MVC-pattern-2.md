---
title: "[MVC] MVC패턴2"
excerpt: "Service, DAO, VO, DTO는 뭘까"

categories:
 - TIL
tags:
 - TIL
 - MVC
 - Java
 - Spring
last_modified_at: 2020-02-03
---



## 그럼 Service, DAO, VO, DTO는 뭘까?

선배들이랑 스프링부트로 프로젝트를 진행할 때, 또 다른 구조를 본적이있다. 이전에 공부한 내용에서는 MVC패턴이면 크게 Model, View, Controller로 나누어 개발하는 디자인인데, 그때는 Service, DTO, VO 패키지도 만들어서 코드를 짰었다. 뭐가 다른걸까?



## Model을 나누었다.

저번 포스팅에서 Model은 데이터를 가공하는 부분이라고 공부했었다. 데이터를 가공하려면 우선 데이터가 필요할것이고, 데이터를 가공하는 로직이 필요할것이다. 회원가입을 예로들면, 회원가입에 필요한 정보들(Id, 패스워드, 이메일 등)이 데이터일것이고, 중복Id 조회, 패스워드 제한 등을 처리해주는 부분이 비즈니스 로직에 해당된다.



 ### DAO(Data Access Object)

DB를 사용하여 데이터를 조회하거나 조작하는 기능을 담당한다. domain logic(비즈니스 로직이나 DB와 관련없는 코드들)을 persistence layer(DB에 data를 CRUD하는 계층)과 분리하기 위해 사용한다.

* 굳이 왜 분리해서 사용할까?

  HTTP Request를 Web Application이 받게되면 쓰레드를 생성하고 쓰레드에서 매번 DB서버와 연결하기 때문에 비용이 많이든다. 따라서 DAO를 만들어서 DB전용 객체로만 사용하면 부담이 줄어들게 된다.

  > 블로그를 참조한 내용이라 이해가 안되는 부분도 많다. 시간날때 더 깊게 공부해봐야할거같다.



### DTO(Data Transfer Object), VO(Value Object)

계층간(Controller, View, Business, Persistence) 데이터 교환을 위한 java beans이다. DB에서 데이터를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체를 말한다. 로직을 가지고 있지 않고 getter/setter/toString 등의 메소드만 가진다.

* VO와 DTO의 차이는 무엇일까?

  동일한 개념이지만 VO는 read only 속성을 가진다.



### Service

Service는 비즈니스 로직이 들어가는 부분이다. Controller가 Request를 받으면 적절한 Service를 전달하고 Request의 데이터들을 DTO와 매핑하여 Service에서 가공하여 반환한다.



---

### 참고자료

https://lazymankook.tistory.com/30

https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html 

