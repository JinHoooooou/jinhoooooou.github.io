---
title: "[Spring boot] TDD로 실제 프로젝트 진행해보기 - Mockito 사용해보기"
excerpt: "Jsoup으로 크롤링하는 시간이 오래걸린다. 테스트트는 빨라야하는데 느린다면 문제가 있다! 해결하기위해 Mock을 사용해보자."
classes: wide
categories:
 - Spring boot
tags:
 - Clean code
 - TDD
 - Java
 - Mock
last_modified_at: 2020-04-23
---



# TDD로 프로젝트 진행하기 3

* TDD로 알라딘에 책이 있는지 없는지 알아보는 메서드를 구현했다.
* 책 제목을 입력하여 어느 책을 찾는지 알 수 있어야하는데 아직 그 단계전에 하드코딩으로 URL을 설정하고 Jsoup을 이용해서 해당 url을 크롤링해서 검색결과가 있는지 없는지만 판단하는 메서드로 구현했다.
* 그런데 테스트가 너무 느리다. 테스트의 장점중 하나가 빠르다는것이다. 빠르게 테스트 결과를 확인 할 수 있고, 맞다면 리팩토링 혹은 기능추가, 틀리다면 코드수정을 할텐데 테스트 결과가 늦게 출력된다.
* 왜 그럴까 생각해보면 크롤링 때문인것 같다. 확실히 크롤링은 많은 시간을 잡아먹는다. 그러면 테스트 할 때마다 크롤링하는것을 기다리고 있어야하나? URL설정을 하드코딩했기 때문에 나는 크롤링 결과를 아는데? 이걸 테스트할때마다 기다려야한다? 상당히 귀찮다.
* 이 시간을 줄이기 위해서 Mock 프레임워크인 Mockito를 사용해보자.

---

## 1. Mockito 사용법

* **1.1 Mock 객체만들기**

  ```java
  import static org.mockito.Mockito.*;
  ...
  List mockList = mock(List.class);
  System.out.println(mockedList.size()); // 0
  ...
  @Mock
  List mockList;
  @Test
  public void example1() {
    MockitoAnnotations.initMocks(this);
  }
  ```

  * Mockito 클래스의 static method인 `mock()`을 이용하여 인터페이스나 클래스를 지정한다.
  * 또는 `@mock`애너테이션을 `MockitoAnnotataions.initMocks(this)`를 이용하여 애너테이션이 지정된 변수들의 mock 객체를 생성한다.
  * 이후부터는 구현 클래스로 객체가 생성된 것처럼 동작한다. 기본 리턴타입 int는 0, boolean은 false, 클래스 타입은 null이다.

* **1.2 테스트 스텁 만들기**

  ```java
  import static org.mockito.Mockito.*;
  ...
  @Test
  public void example() {
    List mockList = mock(List.class);
    when(mockList.get(0)).thenReturn("item");
    when(mockList.size()).thenReturn(1);
    when(mockList.get(1)),thenThrow(new RuntimeExecption());
      
    assertEquals("itme", mockList.get(0));
  }
  ```

  * 위 코드 처럼 지정 메서드에 대한 반환값을 설정 하거나 예외처리를 설정할 수 있다.

* **1.3 검증**

  ```java
  @Test
  public void example() {
    List mockList = mock(List.class);
    mockList.add("item");
    verify(mockList).add("item");
    verify(mockList, times(1)).add("item");
    verify(mockList, never()).add("car");
    verify(mockList, atLeastOnce()).size();
    verify(mockList, atLeast(2)).removeAll();
    verify(mockList, atMost(5)).add("box");
    verify(mocklist, timeout(100).atLeast(1)).size();
  }
  ```

  * `verify()`메서드를 통해 `add("item")`이 호출 되었는지 확인 할 수 있다.
  * 호출 횟수도 지정해서 검증할 수 있다.
    * times(n) : n번 호출되었는지 확인한다.
    * never() : 호출되지 않았는지 확인한다.
    * atLeastOnce() : 최소 한번이상 호출 되었는지 확인한다.
    * atLeast(n) : 최소 n번 호출되었는지 확인한다.
    * atMost(n) : 최대 n이상 호출되었는지 확인한다.
    * timeout(millis) : 지정된 시간 안으로 최소 1번 호출 되었는지 확인한다.

* **1.4 검증 심화 - Argument Matcher**

  ```java
  @Test
  public void example() {
    List mockList = mock(List.class);
    mockList.add("item");
    verify(mockList, times(5)).add(any());
    verify(mockList, atLeastOnce()).get(eq("item"));
  }
  ```

  * 메서드가 몇번 사용는것은 확인 가능한데, 인자까지 특정하려면 Argument matcher를 사용한다.
    * any() : 어떤 객체나 상관없다.
    * anyType() : 해당 타입에 해당
    * anyCollection() : List, Map, Set등 Collection 객체
    * matches(String regax) : 정규식 문자로 찾는다.
    * eq() : 같은 값

---

## 2. 알라딘 프로젝트에 적용

* Given에 있던 `Jsoup.connect(url).get()`을 없애고 Mock객체를 생성한다.

  ```java
  import static org.mockito.Mockito.*;
  
  class BookSearchTest {
    Document mockCrawlingResult = mock(Document.class);
    Elements mockSearch3Result = mock(Elements.class);
    BookSearch booksearch = new BookSearch();
    
    @Test
    @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 status : 404, message : NOT_FOUND를 Json형태로 리턴한다.")
    public void testShouldReturn404AndNotFoundWhenNotExistBookInAladin() throws JSONException {
      // Given: 없는 책을 검색한다. (아무것도 입력안함)
      when(mockCrawlingResult.select("div#Search3_Result"))
        .thenReturn(mockSearch3Result);
      when(mockSearch3Result.size()).thenReturn(0);
     
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(given);
  
      // Then: 404 status와 NOT_FOUND message를 리턴한다.
      assertEquals(404, actual.getInt("status"));
      assertEquals("NOT_FOUND", actual.get("message"));
    }
    @Test
    @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오면 status : 200, message : OK를 Json형태로 리턴한다.")
    public void testShouldReturn200AndOKWhenExistBookInAladin() throws JSONException {
      // Given: 있는 책을 검색한다. ("refactoring")
      when(mockCrawlingResult.select("div#Search3_Result"))
        .thenReturn(mockSearch3Result);
      when(mockSearch3Result.size()).thenReturn(1);
  
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(mockCrawlingResult);
  
      // Then: 200 status와 OK message를 리턴한다.
      assertEquals(HttpStatusCode.OK.statusCode, getStatusCode(actual));
      assertEquals(HttpStatusCode.OK.message, getMessage(actual));
    }
    ...
  }
  ```

  * 실제 코드에서 \<div class="Search3_Result">가 없을 경우 404, 있을 경우 200을 출력해야하기 때문에, 단순히 size()를 0, 1로 mocking을 했다. 
  * 실제로 이렇게 하니 테스트도 통과했고, Jsoup을 이용하여 connect하는 시간도 없어서 테스트가 빠르게 통과했다.
  
* 리팩토링 - Given에서 Mocking하는 부분이 중복되기 때문에 메서드로 추출했다.

  ```java
  import static org.mockito.Mockito.*;
  
  class BookSearchTest {
    
    private static final int RESULT_FAIL_SIZE = 0;
    private static final int RESULT_SUCCESS_SIZE = 1;
    Document mockCrawlingResult = mock(Document.class);
    Elements mockSearch3Result = mock(Elements.class);
    BookSearch booksearch = new BookSearch();
    
    @Test
    @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 status : 404, message : NOT_FOUND를 Json형태로 리턴한다.")
    public void testShouldReturn404AndNotFoundWhenNotExistBookInAladin() throws JSONException {
      // Given: 없는 책을 검색한다. (아무것도 입력안함)
      setGivenMocking(RESULT_FAIL_SIZE);
     
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(given);
  
      // Then: 404 status와 NOT_FOUND message를 리턴한다.
      assertEquals(404, actual.getInt("status"));
      assertEquals("NOT_FOUND", actual.get("message"));
    }
    @Test
    @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오면 status : 200, message : OK를 Json형태로 리턴한다.")
    public void testShouldReturn200AndOKWhenExistBookInAladin() throws JSONException {
      // Given: 있는 책을 검색한다. ("refactoring")
      setGivenMocking(RESULT_SUCCESS_SIZE);
  
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(mockCrawlingResult);
  
      // Then: 200 status와 OK message를 리턴한다.
      assertEquals(HttpStatusCode.OK.statusCode, getStatusCode(actual));
      assertEquals(HttpStatusCode.OK.message, getMessage(actual));
    }
     
    private void setGivenMocking(int searchResultSize) {
      when(mockCrawlingResult.select("div#Search3_Result"))
         .thenReturn(mockSearch3Result);
      when(mockSearch3Result.size()).thenReturn(searchResultSize);
    }
    ...
  }
  ```

---

* 🙋‍♂️ 궁금한것 1. 구글링을 해보니까 Mockito를 라이브러리라고 부르는 사람도 있고 프레임워크라고 부르는 사람이 있다... 라이브러리와 프레임워크 차이가 뭘까?
* 🙋‍♂️ 궁금한것 2. Mockito를 이용해서 Mock 객체를 만들어 테스트 시간을 크게 줄일 수 있었다. 근데 Mockito Mock이 뭘까?