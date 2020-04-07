---
title: "Clean Code 읽기(6)"
excerpt: "3장 함수"
classes: wide
categories:
 - Book
tags:
 - clean code
 - refactoring
last_modified_at: 2020-03-19
---



## 3장 함수

의도를 분명히 표현하는 함수를 어떻게 구현할 수 있을까? 함수에 어떤 속성을 부여해야 읽는 사람이 프로그램 내부를 직관적으로 파악 할 수 있을까?

* 작게 만들어라!

  함수를 만드는 규칙 : 작게! 작게! 더 작게!

  ```java
  public static String renderPageWithSetupsAndTeardowns( PageData pageData, boolean isSuite) throws Exception {
  	boolean isTestPage = pageData.hasAttribute("Test"); 
  	if (isTestPage) {
  		WikiPage testPage = pageData.getWikiPage(); 
  		StringBuffer newPageContent = new StringBuffer(); 
  		includeSetupPages(testPage, newPageContent, isSuite); 
  		newPageContent.append(pageData.getContent()); 
  		includeTeardownPages(testPage, newPageContent, isSuite); 
  		pageData.setContent(newPageContent.toString());
  	}
  	return pageData.getHtml(); 
  }
  ```

  위 예시코드는 충분히 이해할 수 있다. pageData가 Test라면 testPage에 Setup Page와 TearDown Page를 포함한다. 하지만 이보다 더 작게 만들어야한다.

  ```java
  public static String renderPageWithSetupsAndTeardowns( PageData pageData, boolean isSuite) throws Exception { 
     if (isTestPage(pageData)) 
     	includeSetupAndTeardownPages(pageData, isSuite); 
     return pageData.getHtml();
  }
  ```

  * 블록과 들여쓰기

    if/else, while문 등에 들어가는 블록은 한 줄이어야 한다. 즉, 중첩 구조가 생길만큼 함수가 커져서는 안된다는 뜻이다. 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서지 않아야 함수가 읽고 이해하기 쉬워진다.

* 한 가지만 해라!

  함수는 한 가지를 해야하며 그 한가지를 잘 해야한다.

  한가지란 어떤 의미인가? 위 예제는 페이지가 테스트 페이지인지 확인 후 테스트 페이지면 set up과 tear down 페이지를 넣고 html으로 렌더링한다.

  지정된 함수 이름 내에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 한다고 할 수 있다.

  * 함수 내 섹션

    한 함수에서 코드가 섹션별로 구분이 된다면 한 함수 내에서 여러 작업을 한다는 증거이다. 한 작업만 수행하는 함수는 섹션으로 구분하기 어렵다.

* 함수 당 추상화 수준은 하나로!

  추상화 수준이 하나인 단계만 수행한다면 한가지 작업을 한다고 할 수 있다. 그렇다면 추상화 수준은 무엇일까

  `getHtml()`함수는 추상화 수준이 아주 높다. 

  `String pagePathName = PathParser.render(pagepath);`는 추상화 수준이 중간이다.

  `.append("\n")`는 추상화 수준이 매우 낮다.

  한 함수 내에 추사와 수준이 섞이게 되면 코드를 읽는 사람이 헷갈리게된다.

  * 위에서 아래로 코드 읽기 : 내려가기 규칙

    코드는 위에서 아래로 이야기처럼 읽히는것이 좋다. 위에서 아래로 읽으면 함수 추상화 수준이 한 단계씩 낮아지는 코드가 읽기 좋다.

* Switch문

  ```java
  public Money calculatePay(Employee e) throws InvalidEmployeeType {
  	switch (e.type) { 
  		case COMMISSIONED:
  			return calculateCommissionedPay(e); 
  		case HOURLY:
  			return calculateHourlyPay(e); 
  		case SALARIED:
  			return calculateSalariedPay(e); 
  		default:
  			throw new InvalidEmployeeType(e.type); 
  	}
  }
  ```

  switch문(if/else)은 기본적으로 함수 내용이 길어지며, '한 가지' 작업만 수행하지 않는다. 그리고 객체지향의 SRP(Single Responsibility Principle) , OCP(Open Closed Principle)를 위반한다.

  ```java
  public abstract class Employee {
  	public abstract boolean isPayday();
  	public abstract Money calculatePay();
  	public abstract void deliverPay(Money pay);
  }
  -----------------
  public interface EmployeeFactory {
  	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType; 
  }
  -----------------
  public class EmployeeFactoryImpl implements EmployeeFactory {
  	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
  		switch (r.type) {
  			case COMMISSIONED:
  				return new CommissionedEmployee(r) ;
  			case HOURLY:
  				return new HourlyEmployee(r);
  			case SALARIED:
  				return new SalariedEmploye(r);
  			default:
  				throw new InvalidEmployeeType(r.type);
  		} 
  	}
  }
  ```

  switch문을 추상 팩토리에 숨긴다. 팩토리는 switch문을 사용해서 파생 클래스 인스턴스를 생성한다.

  switch문은 불가피하게 써야할 상황이 많으므로, 상황에 따라서는 사용 할 수도있다.

* 서술적인 이름을 사용하라!

  서술적인 이름을 사용하는것이 함수가 하는 일을 좀 더 잘 표현할 수 있다.

  2장에서 의미 있는 이름 관한 얘기를 했는데, 여기서도 마찬가지이다.

* 함수 인수

  함수 인수는 적을 수록 좋다. 인수가 많아질수록 인수를 이해하는데 시간이 들고 결국 이해하기 어려운 함수가 된다.

  출력 인수는 입력 인수보다 이해하기 어렵다. 출력 인수란, return으로 반환받는 값이 아닌 입력 인수가 값이 바뀌는 식의 형태이다. 

  * 많이 쓰는 단항 형식

    1. 인수에 질문을 던지는 경우 : `boolean fileExists("MyFile")`
    2. 인수를 뭔가로 변환해 결과를 반환하는 경우 : `InputStream fileOpen("MyFile")`
    3. 이벤트 함수(입력 인수만 있고 출력인수가 없는 함수)

    위 3가지 경우가 아니라면 단항 함수는 가급적 피한다.

  * 플래그 인수

    플래그 인수는 추하다. 함수 인자로 boolean값을 넘기는것을 삼가자. 그 자체로 이미 여러가지 작업을 한다는 의미이다.

  * 이항 함수

    이항함수는 단항함수보다 이해하기 어렵다. 불가피 한 경우도 생긴다. `Point p = new Point(a,b)`같은 경우. 이항 함수는 그만큼의 위험이 따른다는 사실을 이해하고 사용해야한다. 

  * 삼항 함수

    삼항 함수는 이항 함수보다 훨씬 이해하기 어렵다. 삼항 함수를 만들때는 신중히 고려하고 만들어야한다.

  * 인수 객체

    다항 함수가 필요하다면 객체를 생성하여 인수를 줄이는 방법이 더 좋다.

  * 인수 목록

    `String.format()`처럼 인수 개수가 가변적인 함수(String...같은)도 있다. 

  * 동사와 키워드

    함수의 의도나 인수의 순서와 의도를 제대로 표현하려면 좋은 함수 이름이 필요하다.

    

* 부수 효과를 일으키지 마라!

  부수 효과를 일으키면 시간적인 결합이나 순서 종속성을 초래한다. 함수는 한 가지 작업만 해야한다

  ```java
  public class UserValidator {
  	private Cryptographer cryptographer;
  	public boolean checkPassword(String userName, String password) { 
  		User user = UserGateway.findByName(userName);
  		if (user != User.NULL) {
  			String codedPhrase = user.getPhraseEncodedByPassword(); 
  			String phrase = cryptographer.decrypt(codedPhrase, password); 
  			if ("Valid Password".equals(phrase)) {
  				Session.initialize();
  				return true; 
  			}
  		}
  		return false; 
  	}
  }
  ```

  위 예제 코드에서 `Session.initialize()`는 부수효과를 일으킨다. checkPassword라는 함수에서 세션 초기화라는 다른 작업도 수행한다.

  * 출력 인수

    인수는 함수의 입력으로 해석한다. 출력 인수로 사용하는 코드를 피해야한다. 함수에서 상태를 변경해야 한다면 함수가 속한 객체 상태를 변경하는 방식을 택해라.

* 명령과 조회를 분리하라!

  함수가 하는 작업은 무언가를 수행하거나, 무언가에 답하거나 하나의 작업만 수행해야한다. 둘 다 하면 혼란을 초래한다.

* 오류 코드보다 예외를 사용하라!

  오류 코드 대신 예외를 사용하면 오류 처리 코드가 원래 코드에서 분리되어 깔끔해진다.

  * Try/Catch 블록 뽑아내기

    try/catch 블록은 본래 보기 추하다. 따라서 별도 함수로 뽑아내는 편이 좋다.

  * 오류 처리도 한 가지 작업이다.

    오류 처리 또한 한 가지 작업에 속하므로 오류를 처리하는 함수는 그 작업만 해야한다.

* 반복하지 마라!

  코드 중복은 항상 문제이다. 코드 길이가 늘어날 뿐 아니라 알고리즘이 변하면 모든 코드를 수정해야한다.

* 구조적 프로그래밍

  다익스트라의 구조적 프로그래밍에 다르면 함수 내 모든 블록에 입구와 출구가 하나여야 한다. 따라서 break나 continue를 지양하며 goto는 절대 사용해선 안된다.

* 함수를 어떻게 짜죠?

  단위 테스트, 리팩토링, 중복제거 !









* 추상화 수준이 하나인 함수를 구현하는것이 어렵다고 한다. 실제로 잘 모르겠다. 예시를 들어서 설명해줘서 추상화 수준이 이해가 되기는 하지만 실제로 함수구현할때 어렵다. 예시 코드 3-7를 봤는데 뭔가 이렇게까지 해야하나? 라는 생각이 들기만한다 ㅜㅜ

* Switch문 설명에서 switch를 interface로 구현한것이 그냥 구현한것과 뭐가 다른지 잘 모르겠다. 둘 다 똑같이 switch문을 사용했는데...
* 