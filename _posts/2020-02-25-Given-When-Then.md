---
title: "Given When Then"
excerpt: "테스트 코드의 Given When Then은 뭘까"

categories:
 - TIL
tags:
 - TIL
 - Junit
last_modified_at: 2020-02-25
---





코드리뷰 받으면서 Test Code관한 얘기를 들었는데 Given When Then은 무엇을 나타낼까

* Given : 테스트 수행 이전의 상태를 말하며, Test를 위한 조건이다.
* When : 테스트 할 동작, 메소드를 말한다.
* Then : 테스트로 인해 예상되는 결과이다.



TDD연습을 위해 이 사항을 기본으로하고 계속 연습하는 습관을 가지자.

code wars문제 풀이



[가운데 문자열 추출](https://www.codewars.com/kata/56747fd5cb988479af000028)은 문자열을 입력했을 때

* 문자열 길이가 짝수라면 가운데 2문자를 출력
* 문자열 길이가 홀수라면 가운데 1문자를 출력
* 문자열 길이는 0<length<1000이다.

생각해볼 수 있는 테스트는

1. 문자열 길이가 짝수일 때
2. 문자열 길이가 홀수일 때



```java
@Test
public testShouldReturnMiddleOfOneCharaterWhenStringlengthIsOdd(){
    //Given
    String word = "testing";
    //When
    String actual = Kata.getMiddle(word);
    //Then
    assertEqual("t", actual);
}

@Test
public testShouldReturnMiddleOfTwoCharaterWhenStringlengthIsEven(){
    //Given
    String word = "test";
    //When
    String actual = Kata.getMiddle(word);
    //Then
    assertEqual("es", actual);
}
```

이것을 한번 더 리팩토링하면



```java
private String setWordAndCallGetMiddle(String inputWord){
    String word = inputWord;
    return Kata.getMiddle(word);
}

@Test
public testShouldReturnMiddleOfOneCharaterWhenStringlengthIsOdd(){
    String actual = setWordAndCallGetMiddle("testing")
    //Then : Return middle one character is "t"
    assertEqual("t", actual);
}

@Test
public testShouldReturnMiddleOfTwoCharaterWhenStringlengthIsEven(){
    String actual = setWordAndCallGetMiddle("test")
    //Then : Return middle two character is "es"
    assertEqual("es", actual);
}
```

이런식으로 리팩토링까지 하면 보기 좋다. 이런 연습을 계속 할 수 있도록 하자!



++ 추가 테스트 메소드 naming convention

The article presents a compiled list of unit tests naming strategy that one could follow for naming their unit tests. The article is intended to be a quick reference instead of going through multiple great pages such as following. That said, to know greater details, please feel free access one of these pages listed below and know for yourself.

7가지 naming convention중에 자유롭게(?) 써도 되는듯 하다. 자바 code convention에 가장 부합하는건 3번인거같다.

1. **MethodName_StateUnderTest_ExpectedBehavior**

   * isAdult_AgeLessThan18_False
   * withdrawMoney_InvalidAccount_ExceptionThrown
   * admitStudent_MissingMandatoryFields_FailToAdmit

2. **MethodName_ExpectedBehavior_StateUnderTest**

   * isAdult_False_AgeLessThan18
   * withdrawMoney_ThrowsException_IfAccountIsInvalid
   * admitStudent_FailToAdmit_IfMandatoryFieldsAreMissing

3. **test[Feature being tested]**

   * testIsNotAnAdultIfAgeLessThan18
   * testFailToWithdrawMoneyIfAccountIsInvalid
   * testStudentIsNotAdmittedIfMandatoryFieldsAreMissing

4. **Feature to be tested**

   * IsNotAnAdultIfAgeLessThan18
   * FailToWithdrawMoneyIfAccountIsInvalid
   * StudentIsNotAdmittedIfMandatoryFieldsAreMissing

5. **Should_ExpectedBehavior_When_StateUnderTest**

   * Should_ThrowException_When_AgeLessThan18
   * Should_FailToWithdrawMoney_ForInvalidAccount
   * Should_FailToAdmit_IfMandatoryFieldsAreMissing

6. **When_StateUnderTest_Expect_ExpectedBehavior**

   * When_AgeLessThan18_Expect_isAdultAsFalse
   * When_InvalidAccount_Expect_WithdrawMoneyToFail
   * When_MandatoryFieldsAreMissing_Expect_StudentAdmissionToFail

7. **Given_Preconditions_When_StateUnderTest_Then_ExpectedBehavior**

   * Given_UserIsAuthenticated_When_InvalidAccountNumberIsUsedToWithdrawMoney_Then_TransactionsWillFail

   

