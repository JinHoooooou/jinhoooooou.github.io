---
title: "Clean Code 읽기(7)"
excerpt: "7장 오류 처리"
classes: wide
categories:
 - Book
tags:
 - clean code
 - refactoring
last_modified_at: 2020-03-25
---



## 7장 오류 처리

* 뭔가 잘못될 가능성은 늘 존재하며 그것을 바로 잡을 책임은 프로그래머에게 있다.
* 오류 처리는 중요하지만 오류 처리 코드로 인해 프로그램 논리를 이해하기 어려워진다면 깨끗한 코드라고 부르기 어렵다.
* 우아하고 고상하게 오류를 처리하는 기법과 고려사항을 살펴보자.

#### 오류 코드보다 예외를 사용하라

```java
public class DeviceController {
  ...
  public void sendShutDown(){
    DeviceHandle handle = getHandle(DEV1);
    //디바이스 사애를 점검한다.
    if(handle != DeviceHandle.INVALD) {
      //레코드 필드에 디바이스 상태를 저장한다.
      retrieveDeviceRecord(handle);
      //디바이스가 일시정지 상태가 아니라면 종료한다.
      if(record.getStatus() != DEVICE_SUSPENDED) {
        pauseDevice(handle);
        cleanDeviceWorkQueue(handle);
        closeDevice(handle);
      } else {
        logger.log("Device suspended. Unable to shut down");
      }
    } else {
      logger.log("Invalid handle for : " + DEV1.toString());
    }
  }
  ...
}
```

* 위와 같이 예외를 지원하지 않는 프로그래밍 언어가 많았다. 위와 같은 방법을 사용하면 호출자 코드가 복잡해진다. 함수를 호출한 즉시 오류를 확인해야 하기 때문이다.
* 오류가 발생하면 예외를 던지는 편이 logic과 오류 처리가 뒤섞이지 않아 호출자 코드가 더 깔끔해진다.

```java
public class DeviceController {
  ...
  public void sendShutDown() {
    try {
      tryToShutDown();
    } catch (DeviceshutDownError e) {
      logger.log(e);
    }
  }
  
  private void tryToShutDown() throws DeviceShutDownError {
    DeviceHandle handle = getHandle(DEV1);
    DeviceRecord record = retrieveDeviceRecord(handle);
    pauseDevice(handle);
    cleanDeviceWorkQueue(handle);
    closeDevice(handle);
  }
  
  private DeviceHandle getHandle(DeviceID id) {
    ...
    throw new DeviceShutDownError("Invalid handle for : " + id.toString());
    ...
  }
  ...
}
```

* 단순히 보기만 좋아진것이 아니라 코드 품질도 나아졌다. 디바이스를 종료하는 알고리즘과 오류를 처리하는 알고리즘을 분리했기 때문이다.

#### Try-Catch-Finally 문부터 작성하라

* 예외에서 프로그램 안에 **범위를 정의한다**는 사실은 흥미롭다. try-catch-finally 문에서 try 블록에 들어가는 코드를 실행하면 어느 시점에서든 실행이 중단된 후 catch 블록으로 넘어갈 수 있다.
* try 블록은 트랜잭션과 비슷하다. try 블록에서 무슨 일이 생기든지 catch 블록은 프로그램 상태를 일관성 있게 유지해야 한다. try블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기 쉬워진다.

```java
//테스트 코드
@Test(expected = StorageException.class)
public void retrieveSectionShouldThrowOnInvalidFileName() {
    sectionStore.retrieveSection("invalid - file");
}

//실제 코드
public List<RecordedGrip> retrieveSection(String sectionName) {
    //실제로 구현할 때까지 비어 있는 더미를 반환한다.
    return new ArrayList<RecordedGrip>();
}
```

* 파일이 없으면 예외를 던지는지 알아보는 단위 테스트와 실제 코드이다.  실제 코드가 예외를 던지지 않으므로 단위 테스트는 실패한다. 잘못된 파일 접근을 시도하도록 코드를 변경하면

```java
public List<RecordGrip> retrieveSecion(String sectionName) {
  try {
    FileInputStream stream = new FileInputStream(sectionName);
  } catch (Exception e) {
     throw new StorageException("retrieval error", e);
  }
  return new ArrayList<RecordedGrip>();
}
```

* 코드가 예외를 던지므로 테스트가 성공한다. catch블록에서 예외 유형을 좁혀 FileNotFoundException을 잡아내는 코드로 리팩토링이 가능하다.

```java
public List<RecordGrip> retrieveSecion(String sectionName) {
  try {
    FileInputStream stream = new FileInputStream(sectionName);
    stream.close();
  } catch (FileNotFoundException e) {
     throw new StorageException("retrieval error", e);
  }
  return new ArrayList<RecordedGrip>();
}
```

* 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성하는 방법을 권장한다. 그러면 자연스럽게 try 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션 본질을 유지하기 쉬워진다.

#### 미확인 예외를 사용하라

* 확인된 예외는 OCP(Open Closed Principle - 모든 모듈은 확장에 열려있어야하고, 변경에는 닫혀 있어야한다)를 위반한다. A메서드에서 확인된 예외를 던졌는데 catch블록이 세 단게 위에 있다면 그 사이 메서드 모두 해당 예외를 정의해야한다. 즉, 하위 단계에서 코드를 변경하면 상위 단계 메서드 전부 고쳐야한다. 모듈 로직 코드가 전혀 바뀌지 않았더라도 다시 빌드하고 배포해야한다.
* 모든 상위 메서드가 하위 메서드에서 던지는 예외를 알아야하므로 캡슐화가 깨진다.
* 중요한 라이브러리를 작성하는 등 모든 예외를 잡아야하는 경우도 있다. 이럴 때는 확인된 예외도 유용하다. 그러나 일반적인 애플리케이션은 의존성이라는 비용이 이익보다 크다.

* 🙋‍♂️ 미확인 예외와 확인된 예외
  * 확인된 예외(checked exception)
    * 잘못된 코드가 아닌 잘못된 상황에서 발생하는 예외
    * open file같은 코드로 구현했지만, 외부 환경(not found file)에 다라 발생 할 수 있음
    * 예외처리를 구현하지 않으면 컴파일 에러 발생
  * 미확인 에외(unchecked exception)
    * 런타임 시 잘못 구현된 코드로 인해 발생하는 예외
    * 컴파일 에러가 나지 않지만 예외처리가 없을 경우 프로그램 강제 종료
    * 컴파일 시 확인하지 않음

#### 예외에 의미를 제공하라

* 예외를 던질 때 전후 상황을 충분히 덧붙이면 오류가 발생한 원인과 위치를 찾기가 쉬워진다. 자바는 호출 스택을 제공하지만 실패한 코드의 의도를 파악하려면 오류 메시지에 정보를 담아 예외와 함께 던지는것이 더 좋다.

#### 호출자를 고려해 예외 클래스를 정의하라

* 오류가 발생한 위치나, 오류 유형 등으로 오류를 분류 할 수 있다. 그러나 프로그래머에게 가장 중요한 관심사는 **오류를 잡아내는 방법**이다.

```java
  ACMPort port = new ACMEPort(12);

  try {
    port.open();
  } catch (DeviceResponseExecption e){
    reportPortError(e);
    logger.log("Device response exception", e);
  } catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("Unlock exception", e);
  } catch (GMXError e) {
    reportPortError(e);
    logger.log("Device response exception", e);
  } finally {
    ...
  }
```

* 외부 라이브러리를 호출하는 try-catch-finally 문으로, 외부 라이브러리가 던질 예외를 모두 잡아낸다. 예외에 대응하는 방식이 예외 유형과 무관하게 거의 동일하다. 그래서 간결하게 고치기 쉽다.

```java
  LocalPort port = new LocalPort(12);
  try {
    port.open();
  } catch (PortDeviceFailure e){
    reportPortError(e);
    logger.log(e.getMessage(), e);
  } finally {
    ...
  }
```

* 여기서 LocalPort 클래스는 단순히 ACMEPort 클래스가 던지는 에외를 잡아 변환하는 감싸기 클래스이다.

```java
public class LocalPort {
  
  private ACMEPort innerPort;
  
  public LocalPort(int portNumber) {
    innerPort = new ACMEPort(portNumber);
  }
  
  public void open() {
    try {
      innerPort.open();
    } catch (DeviceResponseException e) {
      throw new PortDeviceFailure(e);
    } catch (ATM1212UnlockedException e) {
      throw new PortDeviceFailure(e);
    } catch (GMMXError e) {
      throw new PortDeviceFailure(e);
    }
  }
  ...
}
```

* LocalPort 클래스처럼 Wrapper 클래스는 매우 유용하다.  실제로 외부 API를 사용할 때는 감싸기 기법이 최선이다. 외부 API를 감싸면 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어든다. 나중에 다른 라이브러리로 갈아타도 비용이 적다. 또한 Wrapper 클래스에서 외부 API를 호출하는 대신 테스트 코드를 넣어주는 방법으로 프로그램을 테스트하기도 쉬워진다. 그리고 API를 설계한 방식에 발목 잡히지 않는다.
* :raising_hand_man: Wrapper 클래스
  * 기본 타입(int, boolean 등) 데이터를 객체로 취급해야 하는 경우가 있다. 기본 타입에 해당하는 데이터를 객체로 포장해주는 클래스를 Wrapper 클래스라고 한다.

#### 정상 흐름을 정의하라

* 충고한 지침들을 잘 따른다면 비즈니스 로직과 오류 처리가 잘 분리된 코드가 나온다. 대부분 깨끗하고 간결한 알고리즘으로 보이지만 오류 감지가 프로그램 언저리로 밀려난다. 

  ```java
  try {
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
  } catch (MealExpenseNotFound e) {
    m_total += getMealPerDiem();
  }
  ```

  * 비용 청구 애플리케이션에서 총계를 계산하는 코드이다. 식비를 비용으로 청구했다면 직원이 청구한 식비를 총계에 더한다. 청구하지 않았다면 일일 기본 식비를 총계에 더한다. 그런데 예외가 논리를 따라가기 어렵게 만든다. 특수 상황을 처리할 필요가 없다면 코드가 더 간결해 질 것이다.
  * `ExpenseReportDAO`를 고쳐 언제나 `MealExpense`객체를 반환하게 한다. 식비를 청구하지 않았다면 일일 기본 식비를 총계에 더하도록 수정하면 catch문을 따로 구현할 필요가 없다.
  * 이를 특수 사례 패턴이라 부른다. 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식이다. 그러면 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다. 클래스나 객체가 예외적인 상황을 캡슐화해서 처리하기 때문이다.

#### null을 반환하지 마라

```java
public void registerItem(Item item) {
  if (item != null) {
    ItemRegistry registry = peristentStore.getItemRegistry();
    if(registry != null) {
      Item existing = registry.getItem(item.getID());
      if(existing.getBillingPeriod().hasRetailOwner()) {
        existing.register(item);
      }
    }
  }
}
```

* 흔히 가지고 있는 null을 반환하는 습관이다. null을 반환하는 코드는 일거리를 늘릴 뿐만 아니라 호출자에게 문제를 떠넘긴다.

* 만약 `peristenceStore`가 null이라면 NullPointerException이 발생할 것이다. 위쪽 어디선가 잡을지도 모르고 아닐지도 모르지만 **나쁘다!**

* null 확인이 누락된 문제라고 말하기 쉽지만 null 확인이 **너무 많아서**문제다. 메서드에서 null을 반환하고픈 유혹이 든다면 그 대신에 예외를 던지거나 특수 사례 객체를 반환한다. 사용하려는 외부 API가 null을 반환한다면 감싸기 메서드를 구현해 예외를 던지거나 특수 사례 객체를 반환한다.

  ```java
  List<Employee> employees = getEmployees();
  if (employees != null) {
     for(Employee e : employees) {
       totalPay += e.getPay()
     }
  }
  
  List<Employee> employees = getEmployees();
  for(Employee e : employees) {
    totalPay += e.getPay()
  }
  ```

  * 아래 코드가 위의 코드보다 깔끔하다. `getEmployees()`메서드는 null을 반환할 수 있지만 메서드 내부에서 null 대신 빈 List를 반환하도록 코드를 수정하면 된다.

#### null을 전달하지 마라

* 메서드가 null을 반환하는 방식도 나쁘지만 메서드로 null을 전달하는 방식은 더 나쁘다. null을 기대하는 API가 아니라면 null을 전달하는 코드는 최대한 피해야한다.

  ```java
  // 1.
  public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
      return (p2.x - p1.x) * 1.5;
    }
    ...
  }
  
  // 2.
  public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
      if(p1 == null || p2 == null) {
        throw InvalidArgumentException();
      }
      return (p2.x - p1.x) * 1.5;
    }
    ...
  }
  
  // 3.
  public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
      assert p1 != null : "p1 should not be null";
      assert p2 != null : "p2 should not be null";
      return (p2.x - p1.x) * 1.5;
    }
    ...
  }
  ```

  * 1번 코드에서 메서드에 null을 전달하면 NullpointerException이 발생한다. 
  * 그래서 2번코드처럼 예외 처리를 한다. NullPointException은 발생하지 않지만 InvalidArgumentException을 잡아내는 처리가 필요하다. 
  * 3번 코드는 assert문을 사용했다. 문서화가 잘 되어 코드 읽기는 편하지만 문제 해결 자체는 못한다. 

* 대부분 프로그래밍 언어는 대부분 호출자가 넘긴 null을 처리하는 방법이 없다.  애초에 null을 넘기지 못하도록 금지하는것이 합리적이다.



---

* :raising_hand_man: Wrapper 클래스 설명부분에서, 보통 Wrapper클래스는 기본 타입을 객체화 하려고 사용하는것이라고 배웠다. 근데 객체에 대해서 Wrapper 클래스를 구현할 수 있나? 본 경험이 없어 잘 와닿지 않는다.
* 구구절절 if문을 사용하여 오류 처리하는 것보다 try catch를 사용하는것이 확실히 코드가 깔끔해서 보기 좋다. 근데 내가 생각했을때 어떤 메서드를 구현할 때 처음부터 예외를 다 생각하기는 힘들다고 생각되기 때문에, 우선 if/else 문으로 구현하고 리팩터링하는 방향이 더 좋아 보인다.