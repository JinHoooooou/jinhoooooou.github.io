---
title: "Clean Code 읽기(7)"
excerpt: "15장 JUnit 들여다보기"
classes: wide
categories:
 - blog
tags:
 - blog
last_modified_at: 2020-03-23
---



## 15장 JUnit 들여다보기

### JUnit 프레임워크

* JUnit4의 ComparisonCompactor 모듈을 깔끔하게 만들어 보자
* ComparisonCompactor는 두 문자열을 받아 차이를 반환한다.

  * [ComparisonCompactorTest](https://github.com/junit-team/junit4/blob/master/src/test/java/junit/tests/framework/ComparisonCompactorTest.java) 테스트 코드
  * [ComparisonCmpactor](https://github.com/junit-team/junit4/blob/master/src/main/java/junit/framework/ComparisonCompactor.java) 소스 코드

* 코드가 매우 좋은 상태로 남아있지만 **보이스카우트 규칙**(처음 왔을 때보다 더 깨끗하게)에 따라 리팩토링해보자.

* 가장 먼저 변수앞에 붙인 접두어 'f'이다. 오늘날 접두어를 굳이 명시할 필요가 없으므로 삭제

* 다음으로 compact 메서드 시작부에 캡슐화되지 않은 조건문이 보인다.

  ```java
  //리팩토링 전
  public String compact(String message) {
    if (expected == null || actual == null || areStringsEqual())
      return Assert.format(message, expected, actual);
    ...
  }
  
  //리팩토링 후
  public String compact(String message) {
    if (shouldNotCompact())
      return Assert.format(message, expected, actual);
    ...
  }
  private boolean shouldNotCompact() {
    return expected == null || actual == null || areStringsEqual();
  }
  ```

  * 의도를 명확히 표현하려면 조건문을 캡슐화 하는것이 좋다. 즉, 조건문을 메서드로 뽑아내 적절한 이름을 붙인다.
  * :raising_hand_man:7장 예외처리에서 null을 전달, 반환하지않는것이 좋다고 본 기억이있는데 더 좋은 방법이 있을까?



