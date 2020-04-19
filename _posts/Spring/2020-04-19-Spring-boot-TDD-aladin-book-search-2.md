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



# TDD로 프로젝트 진행하기 2

* 짝 프로그래밍을 진행했기 때문에 쉽게 느껴진것 일수도 있지만, 어찌되었건 내가 걱정하던 부분과 다르게 의외로 간단하게 TDD로 프로젝트를 진행 할 수 있다. 
* 물론 아직 저번에 생각했던  브라우저에서 "localhost:8080/index.html" url로 request를 보내면 그에 대한 처리를 할 텐데 테스트를 할 수 있나? 에 대한 고민은 여전히 있다. 하지만 일단은 실제 프로젝트를 TDD로 개발하는것을 할 수 있다. 라는것을 느끼기 위해 간단한 프로젝트로 계속 진행해보자.

---

알라딘에서 책 검색을 했을때 결과가 있다면 "Yes", 없다면 "No"를 리턴하도록 했었다. 이것을 Json을 리턴하도록 바꿔보자.

#### 1. 책 검색을 했을 때 검색목록에 결과가 없을 경우 {status : 404, message : NOT_FOUND}를 리턴한다.

* 테스트 코드

  ```java
  @Test
  @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 status : 404, message : NOT_FOUND를 Json형태로 리턴한다.")
  public void testShouldReturn404AndNotFoundWhenNotExistBookInAladin()
      throws IOException, JSONException {
    // Given: 없는 책을 검색한다. (아무것도 입력안함)
    Document given = Jsoup.connect(
        "https://www.aladin.co.kr/search/wsearchresult.aspx?SearchTarget=All&SearchWord=&x=0&y=0")
          .get();
  
    // When: searchBook 메서드를 호출한다.
    JSONObject actual = bookSearch.searchBook(given);
  
    // Then: 404 status와 NOT_FOUND message를 리턴한다.
    assertEquals(404, actual.getInt("status"));
    assertEquals("NOT_FOUND", actual.get("message"));
  }
  ```

* 실제 코드

  ```java
  import org.json.JSONException;
  import org.json.JSONObject;
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public JSONObject searchBook(Document given) throws JSONException {
      JSONObject jsonObject = new JSONObject();
      jsonObject.put("status", 404);
      jsonObject.put("message", "NOT_FOUND");
      return jsonObject;
    }
  }
  ```

* 리팩토링 1 - <div id="Search3_Result"... 태그가 없을경우 해당 json 을 만든 후 리턴한다.

  ```java
  import org.json.JSONException;
  import org.json.JSONObject;
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public JSONObject searchBook(Document given) throws JSONException {
      JSONObject jsonObject = new JSONObject();
      if(given.select("div#Search3_Result").size()==0) {
        jsonObject.put("status", 404);
        jsonObject.put("message", "NOT_FOUND");
      }
      return jsonObject;
    }
  }
  ```



### 2. 책 검색을 했을 때 검색목록에 결과가 없을 경우 {status : 200, message : OK}를 리턴한다.

* 테스트 코드

  ```java
  @Test
  @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 status : 200, message : OK를 Json형태로 리턴한다.")
  public void testShouldReturn200AndOKWhenExistBookInAladin()
        throws IOException, JSONException {
    // Given: 있는 책을 검색한다. ("refactoring")
    Document given = Jsoup.connect(
          "https://www.aladin.co.kr/search/wsearchresult.aspx?SearchTarget=All&SearchWord=refactoring&x=0&y=0")
          .get();
  
    // When: searchBook 메서드를 호출한다.
    JSONObject actual = bookSearch.searchBook(given);
  
    // Then: 200 status와 OK message를 리턴한다.
    assertEquals(200, actual.getInt("status"));
    assertEquals("OK", actual.get("message"));
  }
  ```

* 실제 코드

  ```java
  import org.json.JSONException;
  import org.json.JSONObject;
  import org.jsoup.nodes.Document;
  
  public class BookSearch {
  
    public JSONObject searchBook(Document given) throws JSONException {
      JSONObject jsonObject = new JSONObject();
      if (given.select("div#Search3_Result").size() == 0) {
        jsonObject.put("status", 404);
        jsonObject.put("message", "NOT_FOUND");
      } else {
        jsonObject.put("status", 200);
        jsonObject.put("message", "OK");
      }
      return jsonObject;
    }
  }
  ```



---

* 여기까지가 딱 구현"만" 완료한 것이다. 추가 기능이 여러가지 있을 것이다. 그냥 404, 200 출력하지않고 200일경우 책 목록들.. 해당 책이 있는 지점, 가격 등등도 같이 json형태로 리턴할 수 있을것이다. 짝 프로그래밍으로 완성한 부분은 딱 "있다", "없다"를 판단하는 것이고 나머지 부분은 같이 하던 혼자 하던 TDD로 개발 해보려한다.

* 이전 글이랑 비교해봐도 글 흐름상 리팩토링 단계가 나와야하는데 리팩토링 단계를 따로 빼서 3번 타이틀로 정리하는 이유는 선배들이랑 리팩토링을 하면서 "와 이형들 진짜 잘한다.."라고 크게 느껴서 최대한 자세하게 정리하려한다.

---

### 3. 리팩토링

1. if문에서 else문까지 추가하지 않고, early return하도록 수정한다.

   ```java
   import org.json.JSONException;
   import org.json.JSONObject;
   import org.jsoup.nodes.Document;
   
   public class BookSearch {
   
     public JSONObject searchBook(Document given) throws JSONException {
       JSONObject jsonObject = new JSONObject();
       if (given.select("div#Search3_Result").size() == 0) {
         jsonObject.put("status", 404);
         jsonObject.put("message", "NOT_FOUND");
         return jsonObject;
       } 
       jsonObject.put("status", 200);
       jsonObject.put("message", "OK");
       return jsonObject;
     }
   }
   ```

   * 🙋‍♂️ 이 부분은 그래도 내 수준에서도 쉽게 생각할 수 있는 리팩토링이다.

2. `put()`메서드가 너무 반복된다. 메서드로 추출한다.

   ```java
   public class BookSearch {
   
     public JSONObject searchBook(Document given) throws JSONException {
       JSONObject jsonObject = new JSONObject();
       if (given.select("div#Search3_Result").size() == 0) {
         getJsonFormat(jsonObject, 404, "NOT_FOUND");
         return jsonObject;
       }
       getJsonFormat(jsonObject, 200, "OK");
       return jsonObject;
     }
   
     private void getJsonFormat(JSONObject jsonObject, int statusCode, String message) throws JSONException {
       jsonObject.put("status", statusCode);
       jsonObject.put("message", message);
     }
   }
   ```

   * 🙋‍♂️ 메서드로 추출하는 것도 많이 해봤기 때문에 충분히 생각할 수 있는 리팩토링이다. 여기서 나는 `Clean Code`책에서 본 기억으로 출력인자를 사용하는것은 좋지 않겠다고 생각해서 `jsonObject = getJsonFormat(404, "NOT_FOUND")`로 하는것이 낫지 않을까 생각했었다. 그런데...

3. 바로 Json 객체를 return 하도록 리팩토링한다.

   ```java
   public class BookSearch {
   
     public JSONObject searchBook(Document given) throws JSONException {
       JSONObject jsonObject = new JSONObject();
       if (given.select("div#Search3_Result").size() == 0) {
         return getJsonFormat(404, "NOT_FOUND");
       }
       return getJsonFormat(200, "OK");
     }
   
     private JSONObject getJsonFormat(int statusCode, String message)
         throws JSONException {
       JSONObject jsonObject = new JSONObject();
       jsonObject.put("status", statusCode);
       jsonObject.put("message", message);
       return jsonObject;
     }
   }
   ```

   * 🙋‍♂️ 확실히 이게 더 깔끔한 것 같다. 이 부분에서 첫번째로 "와 이형들 개쩌네"라고 느꼈다.

4. 매직 넘버를 상수로 추출한다.

   ```java
   public class BookSearch {
   
     public static final int OK = 200;
     public static final int NOT_FOUND = 404;
   
     public JSONObject searchBook(Document given) throws JSONException {
       Elements elements = given.select("div#Search3_Result");
       if (elements.size() == 0) {
         return getJsonFormat(NOT_FOUND, "NOT_FOUND");
       }
       return getJsonFormat(OK, "OK");
     }
   
     private JSONObject getJsonFormat(int statusCode, String message)
         throws JSONException {
       JSONObject jsonObject = new JSONObject();
       jsonObject.put("status", statusCode);
       jsonObject.put("message", message);
       return jsonObject;
     }
   }
   ```

   * 🙋‍♂️ OK와, NOT_FOUND에 대한 매직 넘버를 상수로 추출했다 그리고 if문 구절도 너무 길어서 지역변수로 추출했다. 살수로 추출만 하면 된다고 생각했는데, 상수로 추출한것들을 enum클래스나 interface로 빼는것이 더 좋아 보인다고 했다.

5. 상수 클래스 사용 (enum, interface)

   ```java
   //BookSearch.java
   public class BookSearch {
     public JSONObject searchBook(Document given) throws JSONException {
       Elements elements = given.select("div#Search3_Result");
       if (elements.size() == 0) {
         return getJsonFormat(HttpStatusCode.NOT_FOUND.statusCode, "NOT_FOUND");
       }
       return getJsonFormat(HttpStatusCode.OK.statusCode, "OK");
     }
   }
   ...
   
   //HttpStatusCode enum class
   public enum HttpStatusCode {
     NOT_FOUND(404, "NOT_FOUND"),
     OK(200, "OK");
   
     public int statusCode;
     public String message;
   
     HttpStatusCode(int statusCode, String message) {
       this.statusCode = statusCode;
       this.message = message;
     }
   }
   
   //HttpStatusCode interface
   public interface HttpStatusCode {
   
     public static final int NOT_FOUND = 404;
     public static final int OK = 200;
   }
   ```

   * 🙋‍♂️ 선배들이랑 짝프로그래밍으로 할때는 interface를 사용했고 혼자 다시 해볼때는 enum을 사용해보았다.  interface를 사용할 때는 `HttpStatusCode.NOT_FOUND`로 끝낼 수 있어 더 짧게 작성할 수 있다.(내가 enum클래스를 잘 몰라서 그런거일수도..)

6. 명확한 이름사용

   ```java
   package book;
   
   import org.json.JSONException;
   import org.json.JSONObject;
   import org.jsoup.nodes.Document;
   import org.jsoup.select.Elements;
   
   public class BookSearch {
     public JSONObject searchBook(Document crawlingResult) throws JSONException {
       Elements elements = crawlingResult.select("div#Search3_Result");
       if (elements.size() == 0) {
         return getJsonFormat(HttpStatusCode.NOT_FOUND.statusCode, "NOT_FOUND");
       }
       return getJsonFormat(HttpStatusCode.OK.statusCode, "OK");
     }
   
     private JSONObject getJsonFormat(int statusCode, String message)
         throws JSONException {
       JSONObject jsonObject = new JSONObject();
       jsonObject.put("status", statusCode);
       jsonObject.put("message", message);
       return jsonObject;
     }
   }
   
   ```

   * 🙋‍♂️ 짝 프로그래밍으로 할 때는 `Document doc`였는데 명확하지가 않다. crawlingResult로 이름을 지어주면 명확히 알 수 있다.
   * 🙋‍♂️이제 보면 `Element elements...`도 추상화를 고려해서 메서드로 추출 할 수 있을것 같다. `if(!isExistSearchResult(crawlingResult))`이런식으로..?
   * 🙋‍♂️ 이제 "진짜 리팩토링을 끝냈다 이정도면 깔끔한 코드다!" 라고 생각을 했는데 테스트 코드도 리팩토링을 할 수 있다고 했다. "아 테스트 코드도 리팩토링을 해야하는구나.. !"

7. 테스트 코드 리팩토링

   ```java
   class BookSearchTest {
   
     public static final String SUCCESS_URL = "https://www.aladin.co.kr/search/wsearchresult.aspx?SearchTarget=All&SearchWord=refactoring&x=0&y=0";
     public static final String FAIL_URL = "https://www.aladin.co.kr/search/wsearchresult.aspx?SearchTarget=All&SearchWord=&x=0&y=0";
     Document crawlingResult;
     BookSearch bookSearch = new BookSearch();
   
     @Test
     @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오지 않으면 status : 404, message : NOT_FOUND를 Json형태로 리턴한다.")
     public void testShouldReturn404AndNotFoundWhenNotExistBookInAladin()
         throws IOException, JSONException {
       // Given: 없는 책을 검색한다. (아무것도 입력안함)
       crawlingResult = Jsoup.connect(FAIL_URL).get();
   
       // When: searchBook 메서드를 호출한다.
       JSONObject actual = bookSearch.searchBook(crawlingResult);
   
       // Then: 404 status와 NOT_FOUND message를 리턴한다.
       assertEquals(HttpStatusCode.NOT_FOUND.statusCode, getStatusCode(actual));
       assertEquals(HttpStatusCode.NOT_FOUND.message, getMessage(actual));
     }
   
     @Test
     @DisplayName("알라딘 도서 검색창에 책을 검색했을때 결과가 나오면 status : 200, message : OK를 Json형태로 리턴한다.")
     public void testShouldReturn200AndOKWhenExistBookInAladin()
         throws IOException, JSONException {
       // Given: 있는 책을 검색한다. ("refactoring")
       crawlingResult = Jsoup.connect(SUCCESS_URL).get();
   
       // When: searchBook 메서드를 호출한다.
       JSONObject actual = bookSearch.searchBook(crawlingResult);
   
       // Then: 200 status와 OK message를 리턴한다.
       assertEquals(HttpStatusCode.OK.statusCode, getStatusCode(actual));
       assertEquals(HttpStatusCode.OK.message, getMessage(actual));
     }
   
     private int getStatusCode(JSONObject jsonObject) throws JSONException {
       return jsonObject.getInt("status");
     }
   
     private String getMessage(JSONObject jsonObject) throws JSONException {
       return jsonObject.getString("message");
     }
   }
   ```

   * 🙋‍♂️ 테스트 코드도 훨씬 깔끔해졌다. Given, When, Then만 보고도 무엇을 테스트하려 하는지 명확히 파악할 수 있다. 테스트 코드도 이렇게 깔끔하게 리팩토링 하는것이 중요하구나라고 느꼈다.

---

* 이전에 setUp과 tearDown을 간단히 배웠는데, 그 설정도 해줄 수 있을것 같다. 
* 그리고 Jsoup의 connect으로 url에 request를 보내고 response를 받는 시간이 오래걸린다. 테스트는 빨라야하는데 느려진것이 상당히 불만이다. 
* 이를 해결하기 위해서 response를 받았다고 가정하고 테스트를 진행하는 방법이 있다. 이것을 위해 다음에 Mockito 라이브러리를 사용해보려고 한다.

* 짝 프로그래밍을 직접 해보면서 경험해보니 매우 좋다고 느꼈다. 혼자 할 수 있는 일을 왜 굳이 둘이 붙어서 하느냐고 생각했었는데, 직접 해보면서 딴생각이나 쉴 틈없이 계속 일을 하게 되는걸 몸으로 직접 느꼈다. 그리고 막히는 부분이 있으면 옆에서 바로 도움을 주기도 하고, 리팩토링 부분에서도 더 좋은 아이디어를 얘기하면서 하기 때문에 더 깔끔한 코드가 나올 수 있다!

 