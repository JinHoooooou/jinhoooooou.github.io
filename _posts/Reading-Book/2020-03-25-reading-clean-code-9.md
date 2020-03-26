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

  ```java
  //리팩토링 전 클래스 멤버 변수명
  private int fContextLength;
  private String fExpected;
  private String fActual;
  private int fPrefix;
  private int fSuffix;
      
  //리팩토링 후
  private int contextLength;
  private String expected;
  private String actual;
  private int prefix;
  private int suffix;
  ```

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
  
//부정문 -> 긍정문
  public String compact(String message) {
    if (canBeCompacted()) {
        findCommonPrefix();
        ...
        return Assert.format(message, compactExpeted, compactActual);
    } else {
      return Assert.format(message, expected, actual)
    }
  }
  private boolean canBeCompacted() {
      return expected != null && actual != null && !areStringsEquals();
  }
  ```
  
  * 의도를 명확히 표현하려면 조건문을 캡슐화 하는것이 좋다. 즉, 조건문을 메서드로 뽑아내 적절한 이름을 붙인다.
  * 메서드 내에 expected/this.expected와 actual/this.actual을 사용한다. fExpected를 expected로 바꿔버린 바람에 생긴 결과다. 메서드 내 지역변수명을 compactExpected, compactActual로 바꿔준다.
  * 긍정문이 부정문보다 이해하기 더 쉬우므로 if문을 긍정문으로 바꿔준다.
  * :raising_hand_man:7장 예외처리에서 null을 전달, 반환하지않는것이 좋다고 본 기억이있는데 더 좋은 방법이 있을까?

* compact라는 함수명이 이상하다. canBeCompacted()가 false라면 압축하지 않는다. 따라서 compact라고 이름붙이면, 오류 점검하는 부가 단계가 숨겨진다. 그리고 함수는 단순히 압축된 문자열이 아니라 형식이 갖춰진 문자열을 반환한다. 메서드명을 formatCompatedComparison이라고 수정한다.

* if문 안에서 예상 문자열과 실제 문자열을 압축한다. 이 부분을 추출하여 새 메서드로 만든다.

  ```java
  ...
    private String compactExpected;
    private String compactActual;
  ...
    public String formatCompactedComparison(String message) {
      if (canBeCompacted()) {
        compactExpactedAndActual();
        return Assert.format(message, compactExpacted, compactActual);
      } else {
        return Assert.format(message, expected, actual);
      }
  }
  private void compactExpectedAndActual() {
    findCommonPrefix();
    findCommonSuffix();
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
  }
  
  //한번 더 리팩토링
  private void compactExpectedAndActual() {
    prefixIndex = findCommonPrefix();
    suffixIndex = findCommonSuffix();
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
  }
  ```

  * compactedExpected와 compactActual를 클래스 멤버 변수로 올렸다.
  * compactExpectedAndActual()메서드에서 아래의 compactString() 메서드는 변수값을 반환하지만, 위의 findCommonPrefix(), findCommonSuffix()메서드는 반환값이 없어 메서드 사용방식이 일관적이지 못하다. 따라서 반환하도록 바꿔준다. 물론 함수 반환값도 void -> int로 바꾸어준다. prefix, suffix도 결국 index를 나타내므로 변수명을 좀 더 명확하게 한다.
  * :raising_hand_man: 클린 코드 책을 보면서 이런 생각은 한번도 못했는데, 메서드 사용 방식을 일관성있게 하는것도 중요한 대목인것 같다.

* findCommonSuffix메서드를 보면 **숨겨진 시간적인 결합**이 존재한다. 즉, findCommonSuffix는 findCommonPrefix가 prefixIndex를 계산한다는 사실에 의존한다. findCommonPrefix와 findCommonSuffix를 잘못된 순서로 호출하면 고생문이 열린다. 따라서 findCommonSuffix를 고쳐 prefixINdex를 인수로 넘겼다.

  ```java
  //리팩토링 전
  private void compactExpectedAndActual() {
    prefixIndex = findCommonPrefix();
    suffixIndex = findCommonSuffix();
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
  }
  
  //리팩토링 후
  private void compactExpectedAndActual() {
    prefixIndex = findCommonPrefix();
    suffixIndex = findCommonSuffix(prefixIndex);
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
  }
  ```

  * :raising_hand_man:처음에 무슨 말인지 이해가 되지 않았는데 코드를 천천히 읽어보니 findCommonSuffix 메서드의 for문을 살펴보면 prefixIndex가 있다. findCommonPrefix의 연산결과로 prefixIndex값이 정해지고, 그 정해진 값에 따라 findCommonSuffix의 for문을 수행한다.

* prefixIndex를 인수로 전달하는 방식은 메서드 호출 순서는 확실히 정해질지 몰라도 prefixIndex가 필요한 이유가 분명히 드러나지 않는다. 다른 방법을 적용해보자.

  ```java
  private void compactExpectedAndActual() {
    findCommonPrefixAndSuffix();
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
  }
  
  private void findCommonPrefixAndSuffix() {
    findCommonPrefix();
    int expectedSuffix = expected.length() - 1;
    int actualSuffix = actual.length() - 1;
    for (; actualSuffix >= prefixIndex && expectedSuffix >= prefixIndex; actualSuffix--, expectedSuffix--) {
      if (expected.charAt(expectedSuffix) != actual.charAt(actualSuffix)) {
        break;
      }
    }
    suffixIndex = expected.length() - expectedSuffix;
  }
  ```

  * findCommonPrefix와 findCommmonSuffix를 원래대로 되돌리고, findCommonnSuffix라는 이름을 findCommonprefixAndSuffix로 바꾸고, findCommonPrefixAndSuffix에서 가장 먼저 findCommonPrefix를 호출한다. 그러면 함수 호출 순서도 분명해진다.

* findCommonPrefixAndSuffix메서드가 지저분하므로 정리해보자.

  ```java
  //리팩토링 전
  private void findCommonPrefixAndSuffix() {
    findCommonPrefix();
    int expectedSuffix = expected.length() - 1;
    int actualSuffix = actual.length() - 1;
    for (; actualSuffix >= prefixIndex && expectedSuffix >= prefixIndex; actualSuffix--, expectedSuffix--) {
      if (expected.charAt(expectedSuffix) != actual.charAt(actualSuffix)) {
        break;
      }
    }
    suffixIndex = expected.length() - expectedSuffix;
  }
  
  //리팩토링 후
  private void findCommonPrefixAndSuffix() {
    findCommonPrefix();
    int suffixLength = 1;
    for (; !suffixOverlapsPrefix(suffixLength); suffixLength++) {
      if (charFromEnd(expected, suffixLength) !=
          charFromEnd(actual, suffixLength)) {
        break;
      }
    }
    suffixIndex = suffixLength;
  }
  private char charFromEnd(String s, int i) {
    return s.charAt(s.length()-i);
  }
  private boolean suffixOverlapsPrefix(int suffixLength) {
    return actual.length() - suffixLength <= prefixLengh ||
     expected.length() - suffixLength < prefixLength;
  }
  ```

  * :raising_hand_man:리팩토링 계속 하는것을 보면서 느끼는것이, 책에서 제시하는 모든 가이드라인?을 완벽하게 충족시킬수는 없구나 라고 느꼈다. 처음에는  메서드 사용방식을 위해 findCommonPrefix/Suffix메서드 반환형을 정해주었었는데, 다시 없앴다. 그런데 코드는 내가 봐도 더 깔끔하다.

* 코드를 고치고 나니 suffixIndex가 실제로는 suffix의 length를 나타낸다는 것이 드러난다. 통일성을 위해 prefixIndex또한 length로 고쳐준다. suffixIndex는 실제로 0에서 시작하지 않는다. 1에서 시작한다. computeCommonSuffix에 +1이 곳곳에 등장하는 이유이다. 고쳐보자

  ```java
  public class ComparisonCompactor {
    ...
    private int suffixLength;
    ...
    private void findCommonPrefixAndSuffix() {
      findCommonPrefix();
      suffixLength = 0;
      for (; !suffixOverlapsPrefix(suffixLength); suffixLength++) {
        if (charFromEnd(expected, suffixLength) !=
            charFromEnd(actual, suffixLength)) {
          break;
        }
      }
    }
    private char charFromEnd(String s, int i) {
      return s.charAt(s.length() - i - 1);
    }
    private boolean suffixOverlapsPrefix(int suffixLength) {
      return actual.length() - suffixLength <= prefixLengh ||
       expected.length() - suffixLength <= prefixLength;
    }
    ...
    private String compactString(String source) {
      String result = DELTA_START + source.substring(prefixLength, source.length() - suffixLength) + DELTA_END;
      if (prefixLength > 0) {
        result = computeCommonPrefix() + result;
      }
      if (suffixLength > 0) {
        result = result + computeCommonSuffix();
      }
      return result;
    }
    ...
    private String computeCommonSuffix() {
      int end = Math.min(expected.length() - suffixLength + contextLength, expected.length());
      return expected.substring(expected.length() - suffixLength, end) + (expected.length() - suffixLength < expected.length() - contextLength ? ELLIPSIS : "");
      }
    ...     
  }
  ```

  * computeCommonSuffix에서 +1을 없애주고, charFromEnd에 -1 추가하고, suffixOverlapsPrefix에 <=를 추가했다.

* compactString을 보면 suffixLength와 prefixLength를 검사하는 if문이 있으나 마나이다. 고쳐보자.

  ```java
  private String compactString(String source) {
    return computeCommonPrefix() + DELTA_START + source.substring(prefixLength, source.length() - suffixLength) + DELTA_END + computeCommonSuffix();
  }
  ```



* :raising_hand_man: 아직 사소하게 이것저것 손볼 곳이 있고 최종 코드를 봐도 결정 내렸던 부분을 다시 원복한 부분도 있었다. 코드를 리팩토링 하다보면 변경을 원복하는 경우가 흔하다고 한다. 어느 수준에 이를때까지 수많은 시행착오를 반복하는 작업이기 때문이라고한다.
  * 내 생각에 클린코드는 컴퓨터 영역이라기보다 사람의 영역이라고 본다. 그래서 누구에게는 리팩토링한 코드가 깔끔하고 보기 좋을 수 있지만, 또 누구에게는 다른 더 좋은, 더 좋아하는 스타일이 있을거라고 본다. 지금 내 수준에서는 깔끔하게 고치는것은 앚기 어렵기 때문에 이런 리팩토링 예시를 따라하면서 느낌을 익히는 과정이라고 생각한다.
  * 이 과정에서 코드리뷰가 매우 중요할것같다... 내가 리팩토링 하고 리뷰를 해주는 사람이 많으면 더 배울점이 있지 않을까.. 생각해본다. 유튜브 리뷰에서도 많은 사람들한테 깨져봐야한다 했으니..

