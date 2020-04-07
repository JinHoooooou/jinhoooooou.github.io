---
title: "Clean Code 읽기(8)"
excerpt: "10장 클래스"
classes: wide
categories:
 - Book
tags:
 - clean code
 - refactoring
last_modified_at: 2020-03-25
---



## 10장 클래스

함수를 올바로 구현하는 방법과 서로 관련맺는 등 함수에 아무리 신경을 쓰더라도 좀 더 차원 높은 단계까지 신경 쓰지 않으면 깨끗한 코드를 얻기 어렵다. 깨끗한 클래스에 대해 알아보자.

#### 클래스 체계

* 클래스를 정의하는 표준 자바 관례(Convention)에 따르면, 가장 먼저 변수 목록이 나온다. static public 상수가 있다면 가장 먼저나온다. 다음으로 private 변수가 나오며 이어서 인스턴스 변수가 나온다. 변수 목록 이후에 public 함수가 나온다. private 함수는 자신을 호출하는 public 함수 직후에 넣는다. 즉, 추상화 단계가 순차적으로 내려간다. 따라서 프로그램 코드는 신문기사처럼 읽힌다.

##### 캡슐화

* 변수, 유틸리티 함수는 공개하지 않는 편이 낫지만 반드시 숨겨야 한다는 법도 없다. 때로는 protected로 선언하여 테스트 코드가 접근할수있도록 하기도 한다. 하지만 그 전에 비공개 상태를 유지할 방법을 찾는다. 캡슐화 풀어주는 결정은 언제나 마지막 수단이다.

#### 클래스는 작아야 한다!

* 3장 함수에서도 말한것 처럼 클래스 또한 작아야한다. 그렇다면 함수에서 처럼, 얼마나 작아야할까?
  * 함수는 물리적인 행 수로 크기를 측정했다면, 클래스는 클래스가 맡은 책임으로 크기를 측정한다.
* 클래스 이름은 해당 클래스 책임을 기술해야 한다. 실제로 작명은 클래스 크기를 줄이는 첫 번째 관문이다. 간결한 이름이 떠오르지 않는 이유는 그 클래스 크기가 너무 커서 그렇다.  클래스 이름이 Processor, Manager, Super 등 모호한 단어가 있다면 클래스가 여러 책임을 맡았다는 증거다.

##### 단일 책임 원칙

* 단일 책임 원칙(SRP)는 클래스나 모듈을 **변경할 이유**가 단 하나뿐이어야 한다는 원칙이다.

  ```java
  public class SuperDashboard extends JFrame implements MetaDataUser {
    public Component getLastFocusedComponent();
    public void setLastocused(Component lastFocused);
    public int getMajorVersionNumber();
    public int getMinorVersionNumber();
    public int getBuildNumber();
  }
  ```

  * 겉보기에 작아 보이는 위 클래스는 변경할 이유가 두 가지이다.

    * SuperDashboard는 소프트웨어 버전 정보를 추적한다. 그런데 버전 정보는 소프트웨어를 출시할 때마다 달라진다.
    * SuperDashboard는 자바 스윙 컴포넌트를 관리한다. 즉, 스윙 코드를 변경할 때마다 버전 번호가 달라진다.

  * 책임, 즉 변경하 이유를 파악하려 애쓰다 보면 코드를 추상화하기도 쉬워진다. SuperDashboard에서 버전 정보를 메서드를 따로 빼내 Version이라는 클래스를 만들 수 있다.

    ```java
    public class Version {
      public int getMajorVersionNumber();
      public int getMinorVersionNumber();
      public int getBuildNumber();
    }
    ```

* 단일 책임 원칙(SRP)은 객체 지향 설계에서 중요한 개념이지만, 이상하게도 SRP는 클래스 설계자가 가장 무시하는 규칙 중 하나이다. 왜일까?

  * 소프트웨어를 **돌아가게** 만드는 활동과 소프트웨어를 **깨끗하게** 만드는 활동은 완전히 별개이다. 보통 '돌아가는 소프트웨어'에 초점을 맞춘다.
  * 문제는 대다수는 프로그램이 돌아가면 일이 끝났다고 여긴다. '깨끗하고 체계적인 소프트웨어'라는 다음 관심사로 전환하지 않는다.
  * 또한 단일 책임 클래스가 많아지면 큰 그림을 이해하기 어려워진다고 우려한다. 하지만 작은 클래스가 많은 시스템이든 큰 클래스가 적은 시스템이든 돌아가는 부품수는 비슷하다.

* 규모가 어느 수준에 이르는 시스템은 논리가 많고 복잡하다. 이런 복잡함을 다루려면 체계적인 정리가 필수이다. 그래야 개발자가 어디에 무엇이 있는지 쉽게 찾는다.

##### 응집도(Cohesion)

* 클래스는 인스턴스 변수 수가 적어야 한다. 각 클래스 메서드는 클래스 인스턴스 변수를 하나 이상 사용해야 한다. 일반적으로 메서드가 변수를 더 많이 사용 할수록 메서드와 클래스는 응집도가 높다고 할 수 있다. 즉, 응집도가 높다는 말은 클래스에 속하는 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미이다.

  ```java
  public class Stack {
    private int topOfStack = 0;
    List<Integer> elements = new LinkedList<Integer>();
      
    public int size() {
      return topOfStack;
    }
      
    public void push(int element) {
      topOfStack++;
      elements.add(element);
    }
      
    public int pop() throws PoppedWhenEmpty {
      if (topOfStack == 0) {
        throw new PoppedWhenEmpty();  
      }
      int element = elements.get(--topOfStack);
      elements.remove(topOfStack);
      return element;
    }
  }
  ```

  * Stack을 구현한 코드이다. size() 메서드를 제외한 두 메서드는 변수를 모두 사용한다.
  * '함수를 작게, 매개변수 목록을 짧게'라는 전략을 따르다 보면 때때로 몇몇 메서드만이 사용하는 인스턴스 변수가 매우 많아진다. 새로운 클래스로 쪼개야 한다는 신호이다.

##### 응집도를 유지하면 작은 클래스 여럿이 나온다.

* 큰 함수를 작은 함수 여럿으로 나누기만 해도 클래스 수가 많아진다.
* 큰 함수를 작은 함수로 쪼개다보면 클래스가 응집력을 잃는다. 몇몇 함수만 사용하는 인스턴스 변수가 늘어나기 때문이다. 클래스가 응집력을 잃는다면 클래스를 쪼개면 된다. 그러면서 프로그램이 점점 체계가 잡히고 구조가 투명해진다.

```java
public class PrintPrimes {
  public static void main(String[] args) {   
    final int M = 1000; 
    final int RR = 50;
    final int CC = 4;
    final int WW = 10;
    final int ORDMAX = 30; 
    int P[] = new int[M + 1]; 
    int PAGENUMBER;
    int PAGEOFFSET; 
    int ROWOFFSET; 
    int C;
    int J;
    int K;
    boolean JPRIME;
    int ORD;
    int SQUARE;
    int N;
    int MULT[] = new int[ORDMAX + 1];

    J = 1;
    K = 1; 
    P[1] = 2; 
    ORD = 2; 
    SQUARE = 9;

    while (K < M) { 
      do {
           J = J + 2;
           if (J == SQUARE) {
             
             ORD = ORD + 1;
             SQUARE = P[ORD] * P[ORD]; 
             MULT[ORD - 1] = J;
             }
           N = 2;
           JPRIME = true;
           while (N < ORD && JPRIME) {
             while (MULT[N] < J)
               MULT[N] = MULT[N] + P[N] + P[N];
             if (MULT[N] == J) 
               JPRIME = false;
             N = N + 1; 
            }
          } while (!JPRIME); 
      K = K + 1;
      P[K] = J;
    } 
    {
      PAGENUMBER = 1; 
      PAGEOFFSET = 1;
      while (PAGEOFFSET <= M) {
        System.out.println("The First " + M + " Prime Numbers --- Page " + PAGENUMBER);
        System.out.println("");
        for (ROWOFFSET = PAGEOFFSET; ROWOFFSET < PAGEOFFSET + RR; ROWOFFSET++) {
          for (C = 0; C < CC;C++)
            if (ROWOFFSET + C * RR <= M)
              System.out.format("%10d", P[ROWOFFSET + C * RR]); 
          System.out.println("");
        }
        System.out.println("\f");
        PAGENUMBER = PAGENUMBER + 1; PAGEOFFSET = PAGEOFFSET + RR * CC;
      }
    }
  }
}
```

* 함수가 하나뿐인 위 프로그램은 엉망진창이다. 들여쓰기가 심하고, 이상한 변수가 많고, 구조가 빡빡하게 결합되었다. 최소한 여러 함수로 나눠야 마땅하다.

```java
//PrivmePrinter.java
public class PrimePrinter {
  public static void main(String[] args) {
      final int NUMBER_OF_PRIMES = 1000;
      int[] primes = PrimeGenerator.generate(NUMBER_OF_PRIMES);

      final int ROWS_PER_PAGE = 50; 
      final int COLUMNS_PER_PAGE = 4; 
      RowColumnPagePrinter tablePrinter = 
          new RowColumnPagePrinter(ROWS_PER_PAGE, 
                        COLUMNS_PER_PAGE, 
                        "The First " + NUMBER_OF_PRIMES + " Prime Numbers");
      tablePrinter.print(primes); 
  }
}

//RowColumnPagePrinter.java
public class RowColumnPagePrinter { 
  private int rowsPerPage;
  private int columnsPerPage; 
  private int numbersPerPage; 
  private String pageHeader; 
  private PrintStream printStream;

  public RowColumnPagePrinter(int rowsPerPage, int columnsPerPage, String pageHeader) { 
    this.rowsPerPage = rowsPerPage;
    this.columnsPerPage = columnsPerPage; 
    this.pageHeader = pageHeader;
    numbersPerPage = rowsPerPage * columnsPerPage; 
    printStream = System.out;
  }

  public void print(int data[]) { 
    int pageNumber = 1;
    for (int firstIndexOnPage = 0 ; 
      firstIndexOnPage < data.length ; 
      firstIndexOnPage += numbersPerPage) { 
      int lastIndexOnPage =  Math.min(firstIndexOnPage + numbersPerPage - 1, data.length - 1);
      printPageHeader(pageHeader, pageNumber); 
      printPage(firstIndexOnPage, lastIndexOnPage, data); 
      printStream.println("\f");
      pageNumber++;
    } 
  }

  private void printPage(int firstIndexOnPage, int lastIndexOnPage, int[] data) { 
    int firstIndexOfLastRowOnPage =
    firstIndexOnPage + rowsPerPage - 1;
    for (int firstIndexInRow = firstIndexOnPage ; 
      firstIndexInRow <= firstIndexOfLastRowOnPage ;
      firstIndexInRow++) { 
      printRow(firstIndexInRow, lastIndexOnPage, data); 
      printStream.println("");
    } 
  }

  private void printRow(int firstIndexInRow, int lastIndexOnPage, int[] data) {
    for (int column = 0; column < columnsPerPage; column++) {
      int index = firstIndexInRow + column * rowsPerPage; 
      if (index <= lastIndexOnPage)
        printStream.format("%10d", data[index]); 
    }
  }

  private void printPageHeader(String pageHeader, int pageNumber) {
    printStream.println(pageHeader + " --- Page " + pageNumber);
    printStream.println(""); 
  }

  public void setOutput(PrintStream printStream) { 
    this.printStream = printStream;
  } 
}

//PrimeGenerator.java
public class PrimeGenerator {
  private static int[] primes;
  private static ArrayList<Integer> multiplesOfPrimeFactors;

  protected static int[] generate(int n) {
    primes = new int[n];
    multiplesOfPrimeFactors = new ArrayList<Integer>(); 
    set2AsFirstPrime(); 
    checkOddNumbersForSubsequentPrimes();
    return primes; 
  }

  private static void set2AsFirstPrime() { 
    primes[0] = 2; 
    multiplesOfPrimeFactors.add(2);
  }

  private static void checkOddNumbersForSubsequentPrimes() { 
    int primeIndex = 1;
    for (int candidate = 3 ; primeIndex < primes.length ; candidate += 2) { 
      if (isPrime(candidate))
        primes[primeIndex++] = candidate; 
    }
  }

  private static boolean isPrime(int candidate) {
    if (isLeastRelevantMultipleOfNextLargerPrimeFactor(candidate)) {
      multiplesOfPrimeFactors.add(candidate);
      return false; 
    }
    return isNotMultipleOfAnyPreviousPrimeFactor(candidate); 
  }

  private static boolean isLeastRelevantMultipleOfNextLargerPrimeFactor(int candidate) {
    int nextLargerPrimeFactor = primes[multiplesOfPrimeFactors.size()];
    int leastRelevantMultiple = nextLargerPrimeFactor * nextLargerPrimeFactor; 
    return candidate == leastRelevantMultiple;
  }

  private static boolean isNotMultipleOfAnyPreviousPrimeFactor(int candidate) {
    for (int n = 1; n < multiplesOfPrimeFactors.size(); n++) {
      if (isMultipleOfNthPrimeFactor(candidate, n)) 
        return false;
    }
    return true; 
  }

  private static boolean isMultipleOfNthPrimeFactor(int candidate, int n) {
    return candidate == smallestOddNthMultipleNotLessThanCandidate(candidate, n);
  }

  private static int smallestOddNthMultipleNotLessThanCandidate(int candidate, int n) {
    int multiple = multiplesOfPrimeFactors.get(n); 
    while (multiple < candidate)
      multiple += 2 * primes[n]; 
    multiplesOfPrimeFactors.set(n, multiple); 
    return multiple;
  } 
}
```

* 프로그램이 길어 졌다.
  * 리팻터링한 프로그램은 좀 더 길고 서술적인 변수 이름을 사용한다.
  * 리팩터링한 프로그램은 코드에 주석을 추가하는 수단으로 함수 선언과 클래스 선언을 활용한다.
  * 가독성을 높이고자 공백을 추가하고 형식을 맞추었다.
* 프로그램은 세가지 책임으로 나눠졌다. 
  * `PrimePrinter`클래스는 main 함수 하나만 포함하여 실행 환경을 책임진다. 
  * `RowColumnPagePrinter`클래스는 숫자 목록을 주어진 행과 열에 맞춰 페이지에 출력한다.
  * `PrimeGenerator`클래스는 소수 목록을 생성한다. 
    * `PrimePrinter`클래스를 보면 알듯이 객체로 인스턴스화 하지 않고 단순히 변수를 선언하고 감추려고 하는 유용한 공간이다.
* 가장 먼저, 프로그램의 정확한 동작을 검증하는 테스트를 작성한다. 그 다음, 한 번에 하나씩 수 차례에 거쳐 조금씩 코드를 변경했다. 코드를 변경할 때마다 테스트를 수행하여 원래 프로그램과 동일하게 동작하는지 확인한다.

#### 변경하기 쉬운 클래스

* 대다수 시스템은 지속적으로 변경이 가해진다. 변경할 때마다 시스템이 의도대로 동작하지 않을 위험이 따른다. 깨끗한 시스템은 클래스를 체계적으로 정리해 변경에 수반하는 위험을 낮춘다.

```java
public class Sql {
  public Sql(String table, Column[] columns);
  public String create();
  public String insert(Object[] fields);
  public String selectAll();
  public String findByKey(String keyColumn, String keyValue);
  public String select(Criteria criteria);
  public String preparedInsert();
  private String columnlist(Column[] columns);
  private String valuesList(Object[] fields, final Column[] columns);
  private String selectWithCriteria(String criteria);
  private String placeholderList(Column[] columns);
}
```

* 주어진 메타자료로 적절한 SQL문자열을 만드는 Sql 클래스이다. Update 문 같은 기능을 구현하지 않지만 언젠가 추가할 일이 생기면 클래스에 **손대어** 고쳐야한다. 어떤 변경이든 클래스에 손대면 다른 코드를 망가뜨릴 잠정적인 위험이 존재한다. 그래서 테스트도 완전히 다시해야한다.
* 새로운 SQL문을 지원하거나, 기존 SQL문을 수정할때 Sql 클래스를 손대야한다. 변경할 이유가 하나가 아니라면 SRP를 위반하는 것이다.
* 구조적인 관점에서도 `selectWithCriteria`메서드는 select 문을 처리할 때만사용하므로 SRP를 위반한다.
* 클래스에 손대는 순간 설계를 개선하려는 고민과 시도가 필요하다.

```java
//Sql 클래스 변경
abstract public class Sql {
  public Sql(String table, Column[] columns)
  abstract public String generate();
}

public class CreateSql extends Sql {
  public CreateSql(String table, Column[] columns)
  @Override public String generate()
}

public class SelectSql extends Sql {
  public SelectSql(String table, Column[] columns)
  @Override public String generate()
}

public class InsertSql extends Sql {
  public InsertSql(String table, Column[] columns, Object[] fields)
  @Override public String generate()
  private String valueList(Object[] fields, final Column[] columns)
}

public class SelectWithCriteriaSql extends Sql {
  public SelectWithCriteriaSql(String table, Column[] columns, Criteria criteria)
  @Override public String generate()
}

public class SelectWithMatchSql extends Sql {
  public SelectWithMatchSql(String table, Column[] columns, Colum column, String pattern)
  @Override public String generate()
}

public class FindByKeySql extends Sql {
  public FindByKeySql(String table, Column[] columns, String keyColumn, String keyValue)
  @Override public String generate()
}

public class PreparedInsertSql extends Sql {
  public PreparedInsertSql(String table, Column[] columns)
  @Override public String generate()
  private String placeholderList(Column[] columns)
}

public class Where {
  public Where(String criteria)
  public String generate()
}

public class ColumnList {
  public ColumnList(Column[] columns)
  public String generate()
}
```

* Sql 클래스에 있던 공개 인터페이스를 각각 Sql 클래스에서 파생하는 클래스로 만들었다. valueList같은 비공개 메서드는 해당하는 파생 클래스로 옮겼다. 모든 파생 클래스가 공통으로사용하는 비공개 메서드는 Where, ColumnList라는 두 유틸리티 클래스로 옮겼다.
* 각 클래스들은 단순하고 이해하기 쉽다. 함수 하나를 수정한다고 다른 함수가 망가질 위험도 사라졌다. 테스트 관점에서 모든 논리를 구석구석 증명하기도 쉬워졌다. 

* update문을 추가할 때 기존 클래스를 변경할 필요도 없다. 새 클래스 UpdateSql를 Sql클래스를 상속받아 구현하면된다.
* SRP, OCP도 만족한다.

##### 변경으로부터 격리

* 요구사항은 변하기 마련이다. 따라서 코드도 변하기 마련이다.
* 객체 지향 프로그래밍에 구체적인 클래스(concrete)와 추상 클래스(abstract)가 있다.
  * 구체적인 클래스는 상세한 구현을 포함하며 추상 클래스는 개념만 포함한다.
  * 상세한 구현에 의존하는 클라이언트 클래스는 구현이 바뀔 때 위험에 빠지기 때문에 항상 인터페이스와 추상 클래스를 사용하여 구현이 미치는 영향을 격리한다.

* Portfolio 클래스를 만든다고 가정하자. Portfolio 클래스는 외부 TokyoStockExchange API를 사용해 포트폴리오 값을 계산한다. 즉, 우리 테스트 코드는 시세 변화에 영향을 받는다.

  * Portfolio 클래스에서 TokyoStockExchange API를 직접 호출하는 대신 StockExchange라는 인터페이스를 생성 후 메서드 하나를 선언한다.

    ```java
    public interface StockExchange {
      Money currentPrice(String symbol);
    }
    ```

  * Portfolio 생성자에서 StockExchange를 인수로 받는다.

    ```java
    public class Portfolio {
      private StockExchange exchange;
      public Portfolio(StockExchange exchange) {
        this.exchange = exchange;
      }
      //...
    }
    ```

  * 이제 TokyoStockExchange 클래스를 흉내내는 테스트 클래스를 만들 수 있다.

    ```java
    public class PortfolioTest {
      private FixedStockExchangeStub exchange;
      private Portfolio portfolio;
      
      @before
      protected void setUp() throws Exception {
        exchange = new FixedStockExchangeStub();
        exchange.fix("MSFT", 100);
        portfolio = new Portfolio(exchange);
      }
        
      @Test
      public void GivenFiveMSFTTotalShouldBe500() throws Exception {
        portfolio.add(5, "MSFT");
        Assert.assertEquals(500, portfolio.value());
      }
    }
    ```

    * 테스트가 가능할 정도로 시스템 결합도를 낮추면 유연성과 재사용성이 높아진다. 
    * 결합도가 낮다는 소리는 각 시스템 요소가 잘 격리되어 있다는 의미이다.

* 결합도를 최소로 줄이면 자연스럽게 또 다른 클래스 설계 원칙인 DIP(Dependency Inversion Principle)를 따르는 클래스가 나온다.



---

* :raising_hand_man:처음에 Sql 클래스의 공개 인터페이스를 여러 파생 클래스로 나누는 작업을 코드로 봤을때는 이렇게까지 해야하나? 라고 생각했었다. 괜히 코드만 길어진거 같고 한번 슥 읽어봤을때 복잡해보였다. 그런데 이 글을 쓰면서 코드를 직접 타이핑해보니, 확실히 SRP를 잘 지키는 클래스로 바뀌었다. 재사용이나 확장성이 좋아진것을 느낄 수 있었다.
  * 비슷한 예시로 3장 함수 부분에서도 switch/case 문을 interface로 따로 구현한 부분을 봤을때 이해가 잘 안됐었다. 아직도 온전히 이해했다고 볼 수 없지만 확장성 재사용성을 봤을때 그 방법이 맞는것같다.

