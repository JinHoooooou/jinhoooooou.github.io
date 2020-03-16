---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(21)"
excerpt: "중복 제거, clean code, 쿼리 보기"

categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-03-16
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 4-2. 중복 제거, clean code, 쿼리 보기

* 4-3까지 진행을 했으면, 자신의 개인정보 수정 기능까지 구현이 되어있을 것이다.
* 그런데 Controller의 코드를 보면 중복되는부분이 많다. 리팩토링을 해야한다.
  * session을 담당하는 별도의 util 클리스를 만들어볼것이다.

* model이나 session에서 getAttribute 메소드의 이름을 정해줄 때 "user", "completeLoginUser"등 하드코딩 되어있는데 좋은 습관이 아니다. 고쳐보자

  * Util클래스에 상수값으로 뽑아볼것이다.
    * `public static final String USER_SESSION_KEY = "sessionedUser";`
    * 원래는 이렇게하면 mustache 템플릿 문법으로도 중복제거 처리를 해주어야하는데 좀 더 깊이 공부해야하는 부분이 있기때문에 우선은 그대로 둔다.

* 로그인 유무를 판단할 수 있는 메소드를 Util 클래스에 또 따로 뽑아볼것이다.

  * Util클래스는 대체적으로 static으로 구현한다.

  * ```java
    public static boolean isLoginUser(HttpSession session) {
        Object sessionedUser = seesion.getAttribute(USER_SESSION_KEY);
        if(sessionedUser == null) {
            return false;
        }
        return true;
    }
    ```

* 또 User 객체를 써야하기 때문에 getUserFromSession이라는 메소드도 만들어볼것이다.

  * ```java
    public static User getUserFromSession(HttpSession session) {
        if(!isLoginUser(session)) {
            return null;
        }
        return (User) session.getAttribute(USER_SESSION_KEY);
    }
    ```

* Util로 뽑아낸 메소드들을 Controller에 적용해보자
* 비밀번호가 같은지 비교를 많이하는데, 객체지향언어에서 객체에서 get메소드를 통해 비교하는 로직을 많이볼 수 있다. 그런데 그것보다는 객체 자체에서 비교하는 로직을 수행할 수 있도록 설계하는것이 좋다
  * 예를들면 인자로 받은 password == 객체.getPassword 이런식으로 비교하는것 보다 객체.checkSamePassword(인자로받은 password)이런 식으로 하는것이 좋다고한다.
* 비밀번호 체크를 User클래스에서 처리한것처럼 Id도 마찬가지로 User클래스에서 처리하자
* 프로그래밍을 하다보니 아쉬운것이 jpa를 쓰다보니 편하긴한데, 데이터베이스의 쿼리를 확인 할 방법이 없다는것이 좀 아쉽다. 쿼리를 볼 수 있는 방법을 찾아보자
  * properties파일에 spring.jpa.show-sql=true로 해주면된다.
  * 또 spring.jpa.properties.hibernate.format_sql=true도해주자.