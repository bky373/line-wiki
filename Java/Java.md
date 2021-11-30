# Java

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

- **스레드 풀(Thread Pool)**
  - 자바 스레드는 시스템 레벨의 스레드와 매핑되어 있다. 
  - 시스템 레벨의 스레드는 운영 체제의 리소스를 말한다. 
  - 운영 체제는 병렬 처리를 모방하기 위해 스레드 간 컨텍스트 스위칭을 수행한다. 단순하게 생각했을 때 스레드를 더 많이 생성할수록 단일 스레드 작업 수행시간이 줄어든다.
  - 하지만 통제할 수 없을 정도로 스레드를 만들어버리면, 시스템의 리소스는 빠르게 고갈될 것이다.
  - 스레드 풀 패턴을 사용하면, 멀티스레드 환경에서 리소스를 절약하게 해준다. 그리고 미리 정의된 조건 하에서 병렬 처리를 수행할 수 있도록 도와준다.
  - 스레드 풀을 사용할 때는 병렬 작업 형태로 동시성 코드(concurrent code)를 작성하고 이를 스레드 풀의 인스턴스에 넘겨 실행한다. 
  - 이때 스레드 풀 인스턴스는 코드를 실행하는 여러 스레드를 재사용하고 제어한다.
  - 이 패턴을 통해 애플리케이션이 생성하는 스레드의 수와 수명 주기를 제어할 수 있다. 또한 작업 실행을 예약하고, 들어오는 작업을 대기열(큐)에 보관할 수 있다.
  > [출처](https://www.baeldung.com/thread-pool-java-and-guava)

* **H2 데이터베이스 엔진**
  * H2는 자바로 작성된 관계형 데이터베이스 관리 시스템이며, 다음의 주요 기능들을 가지고 있다.
  * 주요 기능
    * 매우 빠른 JDBC API 지원
    * 표준 SQL, JDBC API 지원
    * 자바 애플리케이션에 임베드 하거나 클라이언트-서버 모드로 구동 가능
    * 브라우저 기반의 콘솔 애플리케이션 지원
    * 강력한 보안 기능
    * PostgreSQL ODBC 드라이버 사용 가능
    * 오픈 소스
    * 작은 설치 용량(약 2.5MB jar 파일 크기)
  > [출처](http://www.h2database.com/html/features.html#connection_modes)




# References

- [출처-1](https://java119.tistory.com/52)
