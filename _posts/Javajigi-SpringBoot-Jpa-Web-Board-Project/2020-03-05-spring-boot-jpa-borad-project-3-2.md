---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(13)"
excerpt: "자바와 객체 테이블 매핑, 회원가입 기능 구현"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-05
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 3-1. 자바와 객체 테이블 매핑, 회원가입 기능 구현

* 자바 객체를 데이터베이스 테이블과 매핑 --> 테이블이 자동으로 생성

  * JPA라는 도구를 사용하면 자바 객체에 매핑을 해놓은 설정에따라서 테이블이 자동으로 생성됨

* pom.xml에 spring boot data jpa starter 추가 

* jpa를 활용하면 데이터베이스에 데이터를 추가하고 조회하는것이 편하다. 추후에 직접 데이터베이스에 쿼리를 실행해서 데이터를 넣고 빼는 작업을 하면 jpa가 편하다는것을 느낄 수 있다.

  * 데이터베이스 설정완료

* 그에 맞는 코드를 구현 - package추가

  * 패키지에 User클래스 이동,  User클래스에 annotation을 이용하여 jpa 매핑(@Entity)
  * 고유키(primary key)를 위한 annotation @Id 
  * String을 이용하는것보다 auto incre를 이용한 int값을 사용하는것이 더 좋다. @GeneratedValue
  * userId는 null이되어서는 안된다 --> @Column(nullable=false), length값도 설정가능

* 데이터베이스와 연결을 위해 interface추가 -- UserRepository extends jparepository<Class, ID타입>

  * jparepository를 통해 데이터 삽입, 조회 등 가능

* application.properties파일에 설정 입력

  ```properties
  spring.datasource.url=jdbc:h2:mem:my-slipp;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
  spring.datasource.driver-class-name=org.h2.Driver
  spring.datasource.username=sa
  spring.datasource.password=
  spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
  ```

  

* Controller클래스에 UserRepository를 사용, @Autowired사용

* 테스트

* h2가 메모리 db로 설정되어있기때문에 list에 데이터를 저장한것처럼 서버 재시작을하면 저장한데이터가 없어진다. 그래서 추후에 없어지지않도록 데이터 유지하는방법에대해 알아볼것이다.

---

* 궁금한점

  Autowired를 사용하는 이유