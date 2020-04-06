---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(37)"
excerpt: "중복 제거 및 리팩토링"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-04-06
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 6-5 중복 제거 및 리팩토링

* model 즉 도메인을 먼저 살펴보면 공통적으로 Id, createDate를 가지고있는데 이 부분을 부모 클래스로 추출해볼것이다. 그리고 스프링을 활용하면 생성시간을 자동으로 등록해줄 수 있다.

  ```java
  @MappedSuperclass
  @EntityListeners(AuditingEntityListener.class)
  public class AbstractEntity {
    @Id
    @GeneratedValue
    @JsonProperty
    private Long id;
      
    @CreatedDate
    private LocalDateTime createDate;
    @LastModifiedDate
    private LocalDateTime modifiedDate;
      
    public String getFormattedCreateDate() {
      if(createDate == null) {
        return "";
      }
      return createDate.format(DateTimeFormmater.pattern("yyyy.mm.dd HH:mm:ss"));
    }
  }
  ```

  * jpa에서 부모클래스에 해당하는 클래스를 사용할 때 `@MappedSuperclass` 어노테이션을 추가해준다.
  * 자동으로 데이터가 추가되도록 하기위해 `@CreatedDate` 어노테이션을 추가한다. 마지막 변경시간은 `@LastModifiedDate` 어노테이션을 추가한다.

* `@CreatedDate`와 `@LastModifiedDate`는 추가만하면 되는것이아니라 설정파일에 데이터변경사항을 추적할 수 있도록 해야한다.

  ```java
  @SpringBootApplication
  @EnableJpaAuditing
  public class MySlippApplication {
    ...
  }
  ```

  * `@EnableJpaAuditing`을 추가하여 어노테이션을 인식하여 자동으로 생성시간, 수정시간을 저장한다.

* User, Question, Answer클래스에서 AbstractEntity 클래스를 상속한다.

  ```java
  public class User extends AbstractEntity {
    ...
  }
  
  public class Question extends AbstractEntity {
    ...
  }
  
  public class Answer extends AbstractEntity {
    ...
  }
  ```

* 테스트

> 여기까지 실습