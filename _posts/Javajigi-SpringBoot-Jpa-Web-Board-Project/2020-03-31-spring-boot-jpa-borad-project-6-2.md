---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(34)"
excerpt: "AJAX를 활용한 답변 추가2"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-31
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 6-1. AJAX를 활용한 답변 추가 2

* 응답(response)을 json으로 받는 api를 구현해야하는데 지금 눈으로 바로 확인하기 힘들다. 그래서 사용자 정보를 조회하는 controller를 추가한다음 브라우저상에서 해당하는 사용자의 정보를 json으로 받아보는 기능을 추가해보자.

  ```java
  @RestController
  @RequestMapping("api/users")
  public class ApiUserController {
    @Autowired
    private UserRepository userRepository;
    
    @GetMapping("/{id}")
    public User show(@PathVariable Long id) {
      return UserRepository.findOne(id);
    }
  }
  ```

* 해당 url을 입력하면 json형태의 user정보를 받아올 수 있다.

> 여기까지 실습

* Getter 메서드를 만드는것도 좋지만, @JsonProperty를 추가해줘도 된다. 무시하고 싶은 field가 있다면 @JsonIgnore를 추가해주면된다.

  ```java
  public class User {
    ...
    @JsonProperty
    private Long id;
    @JsonProperty
    private String userId;
    @JsonIgnore
    private String password;
  }
  ```

  * Answer도 마찬가지로 추가해준다.
  * :raising_hand_man: 나는 lombok의 getter를 사용했기 때문에 따로 사용하진않았다.