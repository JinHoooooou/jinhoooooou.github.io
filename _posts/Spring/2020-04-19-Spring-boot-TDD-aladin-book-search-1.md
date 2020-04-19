---
title: "[Spring boot] TDD로 실제 프로젝트 진행해보기 - 알라딘 책 검색"
excerpt: "알고리즘 문제풀때 TDD로 진행했는데 실제 프로젝트에서도 적용할 수 있을까??"
classes: wide
categories:
 - Spring boot
tags:
 - Clean code
 - TDD
 - Java
last_modified_at: 2020-04-19
---



# TDD로 프로젝트 진행하기 1

* TDD의 원리는 간단하다.
  * 반드시 실패하는 단위 테스트 코드를 작성한다.
  * 테스트 코드를 통과하기 위한 최소한의 구현 코드를 작성한다.
  * 구현 코드를 리팩토링한다.
* 3가지 단계를 반복하는 연습을 코드워즈 문제를 풀면서 연습했다. 그런데 실제 프로젝트에서는 어떻게 적용할 수 있을까? 생각해보면 어렵다 잘 모르겠다.
  * 간단한 게시판 구현에 있어서도 Spring boot 프로젝트를 실행했을 때, Controller에서 RequestMapping으로 url설정도 localhost:8080/index.html 등으로 설정 했는데 막상 테스트를 생각해보면 내가 chrome을 켜서 주소창에 url을 입력하여 테스트한다.
  * TDD는 분명 테스트 코드를 먼저 작성하고 구현코드를 작성하는데 이런 테스트들은 브라우저에서 url을 입력해야하고.. 갑자기 혼란이온다..;; ㅜ
* 짝 프로그래밍을 통해 개발자 선배들한테 배운 TDD를 다시 혼자 해보자.



##  알라딘 책 검색

#### 1. 책 검색을 했을 때 검색목록에 결과가 없을 경우

* 우선 프로젝트를 생성하고, BookSearch라는 클래스를 생성하고, 테스트 코드를 작성한다.

* 테스트 코드 1

  ```java
  @Test
  @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 No를 리턴한다.")
  public void testShouldReturnNoWhenNotExistBookInAladin() {
    // Given: 없는 책을 검색한다. (아무것도 입력안함)
      
    // When: searchBook 메서드를 호출한다.
    String actual = bookSearch.searchBook(given);
    // Then: No를 리턴한다.
    assertEquals("No", actual);
  }
  ```

  * 이러고 생각해보니까 없는 책을 검색하려면 어떻게 해야할까?.. 크롤링을 사용해서 해당 페이지를 파싱해야 하면 되지 않을까?

* 테스트 코드 2

  ```java
  @Test
  @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 No를 리턴한다.")
  public void testShouldReturnNoWhenNotExistBookInAladin() throws IOException {
    // Given: 없는 책을 검색한다. (아무것도 입력안함)
    Document given = Jsoup.connect("https://www.aladin.co.kr/search/wsearchresult.aspx?SearchTarget=All&SearchWord=&x=0&y=0").get();
  
    // When: searchBook 메서드를 호출한다.
    String actual = bookSearch.searchBook(given);
    
    // Then: No를 리턴한다.
    assertEquals("No", actual);
  }
  ```
  * Jsoup 라이브러리를 이용해서 해당 url에 대한 페이지를 크롤링했다. Jsoup라이브러리 사용은 나중에 다시 다루고, 일단은 `connect()`메서드와 `get()`메서드를 통해 해당 url를 get방식으로 request를 보낸다고만 알고있자.

* 실제 코드

  ```java
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public String searchBook(Document given) {
  
      return "No";
    }
  }
  ```

* 리팩토링 - 크롤링해온 html 페이지에서 0건의 상품이 검색되었다고 출력이 되면 "No"를 출력하도록 한다.

  ![not-found]({{site.url}}/assets/images/2020-04-19-Spring-boot-TDD-aladin-book-search-1.assets/not-found.png)

  * 0 건의 상품이 검색되었다는 텍스트는 <div class="serach_t_g"... 태그에서 볼 수 있다. Document given을 이용하여 코드를 리팩토링 해보자.

  ```java
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public String searchBook(Document given) {
      if (given.select("div.search_t_g").text().contains("0 건의")) {
        return "No";
      }
      return "";
    }
  }
  ```

  * `select()`메서드를 통해 태그를 가져올 수 있다. 태그 내용을 `text()`메서드와 `contains()`메서드를 통해 "0 건"이라는 string이 있다면 No를 출력하도록 했다.
  * 일단은 이정도로 끝낼 수 있을것 같다. 다음 테스트 고고

### 2. 책 검색을 했을 때 검색목록에 검색 결과가 있을 경우

* "refactoring"이라는 책을 검색했을때 결과창을 보면

  ![ok]({{site.url}}/assets/images/2020-04-19-Spring-boot-TDD-aladin-book-search-1.assets/ok.png)

  * 검색 목록이 출력되며 해당 html 태그는 <div id="Search_Result"...이다. 이 태그는 검색 결과가 있을때만 있는 태그이다. 이제 테스트 코드를 짜보자

* 테스트 코드

  ```java
  @Test
  @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오면 Yes를 리턴한다.")
  public void testShouldReturnYesWhenNotExistBookInAladin() throws IOException {
    // Given: 없는 책을 검색한다. (refactoring)
    Document given = Jsoup.connect("https://www.aladin.co.kr/search/wsearchresult.aspx?SearchTarget=All&SearchWord=refactoring&x=0&y=0").get();
  
    // When: searchBook 메서드를 호출한다.
    String actual = bookSearch.searchBook(given);
  
    // Then: Yes를 리턴한다.
    assertEquals("Yes", actual);
  }
  ```

* 실제 코드

  ```java
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public String searchBook(Document given) {
      if (given.select("div.search_t_g").text().contains("0 건의")) {
        return "No";
      }
      return "Yes";
    }
  }
  ```

* 리팩토링 1 - 검색했을때 결과가 있다면 <div id="Search3_Result".. 태그가 있으니까 이 태그가 존재한다면 Yes를 리턴한다고 해보자

  ```java
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public String searchBook(Document given) {
  
      if (given.select("div.search_t_g").text().contains("0 건")) {
        return "No";
      }
      if(given.select("div#Search3_Result").size()>0) {
        return "Yes";
      }
      return "";
    }
  }
  ```

* 리팩토링 2 - if문을 두가지로 나누지 말고, `select("div#Search3_Result")`의 결과가 없다면 size가 0일 테니까 if문을 줄여보자.

  ```java
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public String searchBook(Document given) {
      if(given.select("div#Search3_Result").size()==0) {
        return "No";
      }
      return "Yes";
    }
  }
  ```



---

* 일단 이정도로도 됐다. 이것만 봤을때는 내가 평소에 코드워즈 문제 풀었을때랑 크게 다른게 없다. 생각해 볼 수 있는것은 테스트 코드에서 주어진 given을 실제 구현 코드에서 url구하는 메서드로 추출할 수 있을것 같다.. 
* 짝 프로그래밍을 할 때, String으로 "Yes", "No" 출력하도록 하지말고 Json형태로 리턴하도록 하자해서 그 부분에 대한 수정을 다음에 해보자.

 