# Lines

## Java

- **Optional 클래스** : `Optional<T>` 클래스는 `Integer`나 `Double` 클래스처럼 `T`타입의 객체를 포장해 주는 **래퍼 클래스**(`Wrapper class`)이다. 따라서 `Optional` 인스턴스는 **모든 타입의 참조 변수**를 저장할 수 있다.
- **JSP** (`Java Server Page`) : HTML내에 자바 코드를 삽입하여 웹 서버에서 **동적으로 웹 페이지를 생성**하여 웹 브라우저에 돌려주는 **서버 사이드 스크립트 언어**이다. Java EE 스펙 중 일부로 웹 애플리케이션 서버에서 동작한다. ( [출처](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%84%9C%EB%B2%84_%ED%8E%98%EC%9D%B4%EC%A7%80) )

- **Java 날짜 및 시간 관련 API  등장 순서**  ( 출처-1 )

  -  `java.util.Date`  ->  `java.util.Calendar`  ->  `java.time(org.joda.time)`
  -  Java 8 부터  `java.time(joda.time) api` 가 출시되어  자바 8 버전 이상 사용 가능

- **LocalDate** : 로컬 날짜 클래스로 **날짜 정보** 만 필요로 할 때 사용한다.

  ```java
  // 로컬 컴퓨터의 현재 날짜 정보를 저장한 LocalDate 객체를 리턴
  LocalDate currentDate = LocalDate.now();
  // 결과 : 2021-07-30
  
  // 파라미터로 주어진 날짜 정보를 저장한 LocalDate 객체를 리턴
  int year = 2021;
  int month = 07;
  int dayOfMonth = 30;
  LocalDate targetDate = LocalDate.of(year, month, dayOfMonth);
  // 결과 : 2021-07-30
  ```

- **LocalTime** : 로컬 시간 클래스로 **시간 정보** 만 필요로 할 때 사용한다.

  ```java
  // 로컬 컴퓨터의 현재 시간 정보를 저장한 LocalDate 객체를 리턴
  LocalTime currentTime = LocalTime.now();
  // 결과 : 14:00:42.695048
  
  // 파라미터로 주어진 시간 정보를 저장한 LocalTime 객체를 리턴
  int hour = 14;
  int minute = 02;
  int second = 33;
  int nanoOfSecond = 77;
  LocalTime targetTime = LocalTime.of(14,00,22);
  // 결과 : 14:02:33.000000077
  ```

- **LocalDateTime** : **날짜와 시간 정보** 모두를 필요로 할 때 사용한다.

  ```java
  // 로컬 컴퓨터의 현재 날짜와 시간 정보
  LocalDateTime currentDateTime = LocalDateTime.now();
  // 결과 : 2021-07-30T14:10:10.088587
  
  // 파라미터로 주어진 날짜 및 시간 정보를 저장한 LocalDateTime 객체를 리턴
  // 파라미터는 위의 이미 작성한 변수들을 사용하였다.
  LocalDateTime localDateTime2 = LocalDateTime.of(year, month, dayOfMonth, hour, minute, second, nanoOfSecond);
  // 결과 : 2021-07-30T14:02:33.000000077
  ```

- 바로 위의 결과 ( `2021-07-30T14:02:33.000000077` ) 에서  중간에 `T` 는 **ISO** 형식의 시간 표기법을 의미한다. 즉,  String  매개변수를  `LocalDateTime` 으로 파싱할 때  T 가 없으면  `java.time.format.DateTimeParseException` 이  발생한다는 뜻이다.

  ```java
  String date1 = "2021-07-30 11:11:11";
  LocalDateTime wrongResult = LocalDateTime.parse(date1);
  // 결과 : java.time.format.DateTimeParseException
  
  String date2 = "2021-07-30T11:11:11";
  LocalDateTime result = LocalDateTime.parse(date2);
  // 결과 : 2021-07-30T11:11:11
  ```

- **날짜 더하기**

  ```java
  LocalDateTime currentDateTime = LocalDateTime.now();
  // 더하기는 plus[~~~]()를 사용하고, 빼기는 minus[~~~]() 를 사용한다.
  LocalDateTime plusDateTime = currentDateTime.plusDays(2);
  // 결과 : 2021-08-01T14:27:38
  ```

- **날짜 비교**

  - 참고로, 나노 초가 있을 경우 나노 초까지 비교한다.

  ```java
  boolean isBefore = startDateTime.isBefore(endDateTime);
  System.out.println(isBefore);
  // 결과 : true
  
  boolean isEqual = startDateTime.isEqual(endDateTime);
  System.out.println(isEqual);
  // 결과 : false
  
  boolean isAfter = startDateTime.isEqual(endDateTime);
  System.out.println(isAfter);
  // 결과 : false
  ```

- **날짜 차이 계산**

  ```java
  LocalDate startDate = LocalDate.now(); // 2021-07-30
  LocalDate endDate = LocalDate.of(2021, 12, 31);
  
  Period period = Period.between(startDate, endDate);
  System.out.println(period);
  // 결과 : P5M1D -> 5개월 1일 차이
  
  System.out.println(period.getYears()); // 결과 : 0
  System.out.println(period.getMonths()); // 결과 : 5
  System.out.println(period.getDays()); // 결과 : 1
  ```

## Software Product

- **JIRA** : 지라(JIRA)는 **아틀라시안**이 개발한 **사유 이슈 추적** 제품이다. **버그 추적**, **이슈 추적**, 프로젝트 관리 기능을 제공하는 소프트웨어이다. Jira는 Java 로 작성되어 있다. ( [출처](https://ko.wikipedia.org/wiki/%EC%A7%80%EB%9D%BC_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)) )

  > 사유 소프트웨어 : 오픈 소스 소프트웨어의 반대말로 클로즈드 소스 소프트웨어(`closed source software`)를 일컫는다. 

- **Kafka** ( 아파치 카프카 ) : Apache Kafka는 실시간으로 기록 스트림을 게시, 구독, 저장 및 처리할 수 있는 분산 데이터 스트리밍 플랫폼입니다. 이는 여러 소스에서 데이터 스트림을 처리하고 여러 사용자에게 전달하도록 설계되었습니다. 간단히 말해, A지점에서 B지점까지 이동하는 것뿐만 아니라 A지점에서 Z지점을 비롯해 필요한 모든 곳에서 대규모 데이터를 동시에 이동할 수 있습니다.  ( [출처](https://www.redhat.com/ko/topics/integration/what-is-apache-kafka) )

- **단위 테스트** : 작성한 코드가 **기대하는 대로 동작**하는지 검증하는 절차

- **JUnit** : Java 기반의 단위 테스트를 위한 프레임워크이다. **Annotation 기반**으로 테스트를 지원하며, **Assert**를 통하여, **(예상, 실제)** 를 비교하여 검증한다.

## Architecture

- **시스템** : **소프트웨어**(프로그램)와 **하드웨어**, **인프라**를 포함하는 개념이다.

- **시스템 아키텍처** : 위의 시스템이 **어떻게 구성되는지**를 다루는 개념이다. 시스템 아키텍처를 올바로 구성하기 위해서 **제약조건**이 무엇인지 명확히 알아야 한다. 

- **3-tier Architecture** : **Presentation**, **Business**, **Data Source**  로 구성되며, 가장 흔하게 사용되는 아키텍처 중 하나이다.

  > **Presentation**: 사용자와 **소통**하는 부분  ( `Front-end` ) <br>
  > **Business** : 사용자가 요청한 것을 **처리**하여 **결과**를 내는 부분 ( `Back-end` ) <br>
  > **Data Source** : 요청으로 처리한 것들이 **저장**되는 부분 ( `Database` ) <br>

## Programming

- **강한 결합** : 객체 내부에서 다른 객체를 생성하는 것은 강한 결합도를 가지는 구조이다. A 클래스 내부에서 B 라는 객체를 직접 생성하고 있다면, B 객체를 C 객체로 바꾸고 싶은 경우에 A 클래스도 수정해야 하는 방식이기 때문에 강한 결합이다. ( [출처](https://devlog-wjdrbs96.tistory.com/165) )

- **느슨한 결합** : 객체를 주입받는다는 것은 외부에서 생성된 객체를 인터페이스를 통해서 넘겨받는 것이다. 이렇게 하면 결합도를 낮출 수 있고, 런타임시에 의존관계가 결정되기 때문에 유연한 구조를 가진다. ( [출처](https://devlog-wjdrbs96.tistory.com/165) )

## Knowledge

- **ISO 날짜 형식 ( ISO 8601 )**   ( 출처-2 )

  - 정식 명칭

    > **Date elements and** **interchange formats** **- Information interchange - Representation of dates and times**

  - **날짜와 시간과 관련된 데이터 교환** 을 다루는 국제 표준이다. 국제 표준화 기구 (`ISO`) 에 의해 공포되었고,  1988년에 처음으로 공개되었다.

  - 날짜 및 시간의 숫자 표현에서 다른 관례를 가진 나라들 간 데이터를 주고 받을 때 숫자 표현에 대한 오해를 줄이기 위해 등장하였다.

  - **가장 기본적인 형식(날짜와 시간)** 은 아래와 같다.

    > 2021-07-30T15:37:00+09:00
    >
    > - 날짜 : 년-월-일
    > - T : 날짜 뒤에 시간이 오는 것을 표시한다.
    > - 시간 : 시:분:초 이나, 프로그래밍 언어에 따라서 초 뒤에 소수점 형태로 milliseconds 가 표시될 수 있다.
    > - Timezone Offset : 시간 뒤에 `+` 또는 `-`  시간:분의 형태로 나온다. UTC 기준 시로부터 얼만큼 차이나는지를 나타낸다. 한국 시간은 UTC 기준 시로부터 + 9시간 의 형태로 나타난다.
    > - Z or +00:00 : UTC 기준 시를 나타내는 표시다.  "+00:00" 으로 나타내기도 한다.

## 임시 공간

- HTTP 파라미터 : 서버로 전달되는 **키와 값의 쌍** 을 말한다. ( `key` : `value` )
- POST 방식은 HTTP 파라미터가 주소창에 노출되지 않는다. 즉, 주소창의 변화 없이 데이터만 서버로 전송할 수 있다.



# References

- [출처-1](https://java119.tistory.com/52)
- [출처-2](https://java119.tistory.com/24)
