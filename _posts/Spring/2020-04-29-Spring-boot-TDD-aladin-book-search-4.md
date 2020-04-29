---
title: "[Spring boot] TDD로 실제 프로젝트 진행해보기 - 책 제목 출력"
excerpt: "Mocking을 이용해서 테스트를 빠르게 할 수 있게 되었다. 이제 책 검색 결과가 있다면 title을 출력해보자."
classes: wide
categories:
 - Spring boot
tags:
 - Clean code
 - TDD
 - Java
 - Mock
last_modified_at: 2020-04-29
---



# TDD로 프로젝트 진행하기 4

* Mockito 프레임워크를 이용해서 크롤링하는 시간을 없애 테스트를 빠르게 진행했다.
* 이전에는 200 OK, 404 NOT_FOUND만 Json에 담았었는데 이제 200 OK일 경우 책 제목을 Json에 담아보자.

---

## 1. 검색 결과가 없을때 empty list를 추가한다.

* **테스트 코드**

  ```java
  class BookSearchTest {
  
    private static final int RESULT_FAIL_SIZE = 0;
    private static final int RESULT_SUCCESS_SIZE = 1;
    Document mockCrawlingResult = mock(Document.class);
    Elements mockSearch3Result = mock(Elements.class);
    BookSearch bookSearch = new BookSearch();
    
    ...
      
    @Test
    @DisplayName("결과 값이 없는 경우 bookList는 empty list이어야 한다")
    public void testShouldReturnEmptyBookListWhenStatusIs404() throws JSONException {
      // Given: 실패하는 검색 결과 세팅 (아무것도 입력 안함)
      setGivenMocking(RESULT_FAIL_SIZE);
  
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(mockCrawlingResult);
  
      // Then: "bookList"의 value는 empty list
      assertEquals(0, getBookList(actual).length());
    }
    
    private JSONArray getBookList(JSONObject jsonObject)
      throws JSONException {
      return jsonObject.getJSONArray("bookList");
    }
    
    ...
  }
  ```
  
* **실제 코드**

  ```java
  public class BookSearch {
  
    public JSONObject searchBook(Document crawlingResult)
      throws JSONException {
      Elements Search3_Result = crawlingResult.select("div#Search3_Result");
      JSONArray bookList = new JSONArray();
      if (Search3_Result.size() == 0) {
        return getJsonFormat(HttpStatusCode.NOT_FOUND.statusCode,
                             HttpStatusCode.NOT_FOUND.message,
                             bookList);
      }
      return getJsonFormat(HttpStatusCode.OK.statusCode,
                           HttpStatusCode.OK.message,
                           bookList);
    }
  
    private JSONObject getJsonFormat(int statusCode, String message, JSONArray bookList)
        throws JSONException {
      JSONObject jsonObject = new JSONObject();
      jsonObject.put("status", statusCode);
      jsonObject.put("message", message);
      jsonObject.put("bookList", bookList);
      return jsonObject;
    }
  }
  ```

  * 단순히 `getJsonFormat()`메서드의 매개 변수로 bookList를 추가했다. 추가하고 element이름이 애매해서 "Search3_Result"로 바꿨다.

---

## 2. 검색 결과가 있을 때 "bookList" value에 title을 담는다.

* **테스트 코드**

  ```java
  class BookSearchTest {
  
    private static final int RESULT_FAIL_SIZE = 0;
    private static final int RESULT_SUCCESS_SIZE = 1;
    Document mockCrawlingResult = mock(Document.class);
    Elements mockSearch3Result = mock(Elements.class);
    BookSearch bookSearch = new BookSearch();
    
    ...
      
    @Test
    @DisplayName("검색 결과가 있다면 Json에 bookList를 추가한다.")
    public void testShouldReturnBookListWhenSearchResultExist() throws JSONException {
      // Given: 성공하는 검색 결과 세팅 ("refactoring")
      setGivenMocking(RESULT_SUCCESS_SIZE);
  
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(mockCrawlingResult);
  
      // Then: actual의 "bookList"의 value는 3개이다.
      assertEquals("리팩터링 2판 (리팩토링 개정판)", getBookList(actual).get(0));
      assertEquals("리팩토링 자바스크립트 Refactoring JavaScript", getBookList(actual).get(1));
      assertEquals("Refactoring: Improving the Design of Existing Code (Hardcover, 2)",
          getBookList(actual).get(2));
    }
    
    ...
  }
  ```

  * 이렇게 하고나서 mocking하여 title을 삽입하려 했다. 알라딘 검색창에 "refactoring"을 검색하고 개발자 도구에서 title이 작성된 html 태그를 살펴보니

    ![Success]({{site.url}}/assets/images/2020-04-29-Spring-boot-TDD-aladin-book-search-4.assets/Success.png)

  * \<b>태그였다. 근데 Jsoup을 이용해서 mocking을 하려니 계속 mock객체를 만들어내야하는 어려움이 있었다. 그래서 선배들의 조언을 들어 mockito 라이브러리를 사용하지않고, 내가 mock객체를 만들었다.

* **mock 객체를 위한 html tag String**

  ```java
  public interface MockParsingData {
  
    public static final String FAIL_RESULT_DOCUMENT =
      "<div class=\"ss_center Search3NoResult\" style=\"\">\n"
      ...;
  
    public static final String SUCCESS_RESULT_DOCUMENT =
      "<div id=\"Search3_Result\" style=\"padding: 10px 0px 0px 0px;\">"
      ...;
  }
  ```

  * 검색 결과가 있을 때("refactoring"검색) 책 목록은 3개만 추가했다.("리팩터링 2판 (리팩토링 개정판", "리팩토링 자바스크립트 Refactoring JavaScript", "Refactoring: Improving the Design of Existing Code (Hardcover, 2)")
  * [commit 참고](https://github.com/JinHoooooou/Aladin-Search-Book/commit/64a202958c20756a639974264c41832fe6e022c0)

* **다시 테스트 코드**

  ```java
  class BookSearchTest {
  
    Document mockCrawlingResult;
    BookSearch bookSearch = new BookSearch();
  
    @Test
    @DisplayName("검색 결과가 없다면 Json에 empty bookList를 추가한다.")
    public void testShouldReturnEmptyBookListWhenSearchResultIsNotExist() throws JSONException {
      // Given: 실패하는 검색 결과 세팅 (아무것도 입력 안함)
      mockCrawlingResult = Jsoup.parseBodyFragment(MockParsingData.FAIL_RESULT_DOCUMENT);
  
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(mockCrawlingResult);
  
      // Then: actual의 "bookList"의 value는 empty list이다.
      assertEquals(0, getBookList(actual).length());
    }
  
    @Test
    @DisplayName("검색 결과가 있다면 Json에 bookList를 추가한다.")
    public void testShouldReturnBookListWhenSearchResultExist() throws JSONException {
      // Given: 성공하는 검색 결과 세팅 ("refactoring")
      mockCrawlingResult = Jsoup.parseBodyFragment(MockParsingData.SUCCESS_RESULT_DOCUMENT);
  
      // When: searchBook 메서드를 호출한다.
      JSONObject actual = bookSearch.searchBook(mockCrawlingResult);
  
      // Then: actual의 "bookList"의 value는 3개이다.
      assertEquals("리팩터링 2판 (리팩토링 개정판)", getBookList(actual).get(0));
      assertEquals("리팩토링 자바스크립트 Refactoring JavaScript", getBookList(actual).get(1));
      assertEquals("Refactoring: Improving the Design of Existing Code (Hardcover, 2)",
          getBookList(actual).get(2));
    }
    ...
  }
  ```

  * [commit 참고](https://github.com/JinHoooooou/Aladin-Search-Book/commit/5fb9d7b5c8749d76a26ed955c879429997ad1006)
  * mockito 라이브러리를 사용하지 않고, `Jsoup.parseBodyFragment()`메서드를 통해 mock 객체를 만들었다.
  * 테스트 속도도 `Jsoup.conncet()`처럼 느리지않고 빨랐다.

* **실제 코드**

  ```java
  public class BookSearch {
  
    public JSONObject searchBook(Document crawlingResult) throws JSONException {
      Elements Search3_Result = crawlingResult.select("div#Search3_Result");
      JSONArray bookList = new JSONArray();
      if (Search3_Result.size() == 0) {
        return getJsonFormat(HttpStatusCode.NOT_FOUND.statusCode, HttpStatusCode.NOT_FOUND.message,
            bookList);
      }
      Elements booksTitle = Search3_Result.select("a.bo3 > b");
      for (Element bookTitle : booksTitle) {
        bookList.put(bookTitle.text());
      }
      return getJsonFormat(HttpStatusCode.OK.statusCode, HttpStatusCode.OK.message, bookList);
    }
  
    private JSONObject getJsonFormat(int statusCode, String message, JSONArray bookList)
        throws JSONException {
      JSONObject jsonObject = new JSONObject();
      jsonObject.put("status", statusCode);
      jsonObject.put("message", message);
      jsonObject.put("bookList", bookList);
      return jsonObject;
    }
  }
  ```

  * \<b>태그만 찾으려고 하니 수많은 \<b>태그가 있어서 \<a class="bo3">태그 내에 있는 \<b>태그를 찾기위해 `select("a.bo3 > b")`를 사용했다. [Jsoup 공식문서](https://jsoup.org/cookbook/extracting-data/dom-navigation)를 참고했다.

---

* 🙋‍♂️ Json 구조에 대해 이해를 잘못한 것과, JSON API 사용을 처음 해봐서 혼선이 있었다.

  ```json
  {
    "status" : 200,
    "message" : "OK",
    "bookList" : ["title1", "title2", "title3"]
  }
  ```

  * 이런 구조에서도 `JSONArray.put()`메서드를 사용할 수 있다는것을 알았다.

  ```json
  {
    "status" : 200,
    "message" : "OK",
    "bookList" : 
    [{
      "title" : "책 제목1",
      "wrtier" : "책 저자1"
     },
     {
      "title" : "책 제목2",
      "writer" : "첵 저자2" 
    }]
  }
  ```

  * 나는 이런 구조에서만 `JSONArray.put()`을 사용해야 하는줄 알았다. 그래서 `JSONObject.put()`메서드에 value값을 String[]으로 추가했다. 그래서 테스트 과정에서도 에러도 계속나고 실패해서 구글링을 해보니  `JSONArray.put()`을 사용해도 된다는 것을 알았다.
  * 이런 일련의 과정이 삽질이긴한데.. 그래도 이런 과정을 통해 하나라도 더 알게된게 어디냐..라고 위로해야지 ㅋㅋㅋ ㅜㅜㅜ

  

