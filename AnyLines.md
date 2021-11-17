# AnyLines

## Software Product

- **JIRA** : 지라(JIRA)는 **아틀라시안**이 개발한 **사유 이슈 추적** 제품이다. **버그 추적**, **이슈 추적**, 프로젝트 관리 기능을 제공하는 소프트웨어이다. Jira는 Java 로 작성되어 있다. ( [출처](https://ko.wikipedia.org/wiki/%EC%A7%80%EB%9D%BC_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)) )

  > 사유 소프트웨어 : 오픈 소스 소프트웨어의 반대말로 클로즈드 소스 소프트웨어(`closed source software`)를 일컫는다. 

- **단위 테스트** : 작성한 코드가 **기대하는 대로 동작**하는지 검증하는 절차

- **JUnit** : Java 기반의 단위 테스트를 위한 프레임워크이다. **Annotation 기반**으로 테스트를 지원하며, **Assert**를 통하여, **(예상, 실제)** 를 비교하여 검증한다.

- **ActiveMQ**  ( [출처](https://dev-jj.tistory.com/entry/MQ-Message-queue%EB%9E%80) )

  - 아파치 ActiveMQ는 **가장 널리 사용되는 오픈소스, 다중 프로토콜, Java 기반 메시지 브로커** 이다.
  - 업계 표준 프로토콜을 지원하므로 사용자는 광범위한 언어 및 플랫폼에서 클라이언트 선택의 이점을 얻을 수 있다. (JavaScript, C, C++, Python, .Net 등)
  - 유비쿼터스 AMQP 프로토콜을 사용하여 다중 플랫폼 애플리케이션을 통합할 수 있다.
  - 웹 소켓을 통해 STOMP를 사용하여 웹 애플리케이션 간 메시지를 교환할 수 있다.
  - MQTT를 사용하여 IoT 장치를 관리할 수 있다.
  - ActiveMQ는 기존 JMS 인프라 및 그 이상을 지원하며, 모든 메시징 유즈케이스를 지원하는 기능과 유연성을 제공한다.

- **JMS(Java Message Service)**  ( [출처](https://www.ibm.com/docs/ko/cics-ts/5.6?topic=server-java-message-service-jms) )

  - JMS는 Java EE에 기반한 애플리케이션 구성요소에서 **메시지를 작성, 전송, 수신하고 읽을 수 있도록 하는 API** 이다. 


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

- **UTC** ( `협정 세계시` )   ( 출처-3 )

  - 협정 세계시는 1972년 1월 1일부터 시행된 **국제 표준시** 이다.

  - 영어로는 ( `Coordinated Universal Time` ), 프랑스어로는 ( `Temps Universal Coordonné` ) 인데, 둘의 공통점을 모아 UTC 로 사용하고 있다. ( 다른 약어들과의 일관성을 유지하기 위해 U 가 먼저 오게 됨 )

  - UTC 는 그리니치 평균시(GMT) 라고도 부르며, UTC 와 GMT 는 초의 소숫점 단위에서만 차이나기 때문에 일상에서는 혼용하여 사용한다. 기술적인 표준에서는 UTC 가 사용된다.

    > 참고로, 그리니치 평균시 ( Greenwich Mean Time,  GMT ) 는 런던(영국의 수도)을 기점으로 하고 웰링턴(뉴질랜드의 수도)을 종점으로 하는 협정 세계시의 기준 시간대이다.

  - 협정 세계시는 그레고리력의 표기를 따른다. 1일은 24시간으로 나뉘고, 1시간은 60분으로 나뉜다. 1분은 60초로 나뉘는 것이 보통이나 약간은 가변적이다. 지구의 자전 주기가 일정하지 않기 때문에 UTC에서는 때때로 하루의 제일 마지막 1분을 61초로 계산한다. 이렇게 추가되는 초를 윤초라고 하고, 주로 12월 31일이나 6월 30일의 마지막에 추가한다. 

    > 그레고리력 : 현재 전 세계적으로 통용되는 양력

- **CSRF** 

  - Cross-Site Request Forgery
  - 특정 사용자가 아닌 불특정 다수를 대상으로 하며, 로그인한 사용자는 자신의 의지와는 무관하게 공격자가 의도한 행위(데이터 수집, 삭제, 등록 등)를 하게 된다.

## 프로그래밍 패러다임

- **OOP(객체 지향 프로그래밍)**
  - 객체 지향 프로그래밍 (Object-Oriented Programming)은 프로그래밍 패러다임 중 하나이다.
  - 컴퓨터 프로그램을 명령어의 목록에서 보는 시각에서 벗어나 다양한 독립체(객체)들의 상태와 관계의 모음으로 보는 것을 의미한다.
  - 각각의 객체는 메시지를 주고받으며, 데이터를 처리한다.
  - 직관적인 코드 분석을 가능하고, 유연하고 변경이 쉽기 때문에 대규모 소프트웨어 개발에 많이 사용된다.

  > [출처](https://ko.wikipedia.org/wiki/%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)


- **SOLID(객체 지향 설계)**
  - 유지 보수와 확장에 용이한 시스템을 개발하고자 할 때 적용하는 원칙들
  
  | 첫글자 | 약어 | 개념                                                         |
  | ------ | ---- | ------------------------------------------------------------ |
  | S      | SRP  | 단일 책임 원칙 (Single Responsibility Principle)<br />- "한 클래스는 하나의 책임만 가져야 한다." |
  | O      | OCP  | 개방-폐쇄 원칙 (Open/Closed Principle)<br />- "소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다." |
  | L      | LSP  | 리스코프 치환 원칙 (Liskov Substitution Principle)<br />- "프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다." |
  | I      | ISP  | 인터페이스 분리 원칙 (Interface Segregation Principle)<br />- "특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다." |
  | D      | DIP  | 의존관계 역전 원칙 (Dependency Inversion Principle)<br />- "프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안 된다." 의존성 주입은 이 원칙을 따르는 방법 중 하나이다. |
  > [출처](https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84))

- **함수형 프로그래밍**

  - Functional Programming
  - 프로그래밍 패러다임 중 하나로, 자료 처리를 수학 함수의 연산을 사용하듯이 수행함과 동시에, 상태와 가변 데이터를 취급하지 않는 특징을 가진다.
  - 프로그래밍이 문의 형태가 아닌 식이나 선언으로 수행되는 선언형 프로그래밍 패러다임을 따른다.
  - 함수형 코드는 명령형 코드와 달리 출력값이 오직 함수에 입력된 인수에만 의존한다. 따라서 인수 x에 같은 값을 넣고 함수 f를 호출하면 항상 f(x)라는 결과가 나온다. 이로써 프로그램의 동작을 이해하고 예측하기가 훨씬 쉽다.

  > [출처](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

## 임시 공간

- HTTP 파라미터 : 서버로 전달되는 **키와 값의 쌍** 을 말한다. ( `key` : `value` )

- POST 방식은 HTTP 파라미터가 주소창에 노출되지 않는다. 즉, 주소창의 변화 없이 데이터만 서버로 전송할 수 있다.

- **JSP**

  -  `JSP`는  `Java Server Pages` 의 약자로 웹사이트를 보다 쉽게 구축할 수 있게 만들어 주는, **자바 기반 서버 사이드 ( `Server Side` )  스크립트 언어** 이다. 

- **Pageable** ( [출처](http://devstory.ibksplatform.com/2020/03/spring-boot-jpa-pageable.html) )

  - `org.springframework.data.domain.Pageable` 

    > Abstract interface for pagination information.
    >
    > 페이징 정보를 제공하는 추상 인터페이스

  - `Pageable` 을 사용하여 아래와 같은 이점을 얻을 수 있다.

    - 요건에 맞는 `Pagination`을 구현할 수 있다.
    - 정렬이 필요한 데이터를 쉽게 `Sorting` 할 수 있다.

- **Spring REST Docs**  ( [출처](https://subji.github.io/posts/2021/01/06/springrestdocsexample) )

  - RESTful 서비스의 문서화를 도와주는 도구이다. 문서 작성 도구로 기본적으로  [Asciidoctor](https://asciidoctor.org/) 를 사용하며,  [Asciidoctor](https://asciidoctor.org/)는 쉽게 텍스트 처리하여 HTML 등의 문서를 만들어주는 툴이다. Spring REST Docs도 이것을 사용해 HTML 을 생성한다. 필요에 따라 Markdown 으로 변경할 수 있다.
  - Spring REST Docs 는 Spring MVC 의 [테스트 프레임 워크](https://docs.spring.io/spring-framework/docs/5.0.x/spring-framework-reference/testing.html#spring-mvc-test-framework), Spring WebFlux WebTestClient 또는 [Rest Assured 3](http://rest-assured.io/) 로 작성된 테스트 코드에서 생성된 Snippet 을 사용한다. **테스트 기반 접근 방식은 서비스 문서의 정확성을 보장해준다.** Snippet 이 올바르지 않을 경우 테스트가 실패하기 때문이다.

- **RFC( `Request for Comments` ) 문서**  ( [출처](https://ko.wikipedia.org/wiki/RFC) )

  - 컴퓨터 네트워크 공학 등에서 인터넷 기술에 적용 가능한 새로운 연구, 혁신, 기법 등을 아우르는 메모이다. 동시에 **비평을 기다리는 문서** 라는 의미를 갖는다.
  - 인터넷 협회(Internet Society)에서 **기술자 및 컴퓨터 과학자들은 RFC 메모의 형태로 생각을 출판** 한다. 이러한 출판의 목적은 자신의 새로운 생각 및 정보에 대해 전문가 비평을 바라는 것, 혹은 그러한 생각을 단순히 전달하는 것이다. 인터넷국제표준화기구(IETF)는 일부 RFC를 인터넷 표준으로 받아들이기도 한다.
  - RFC 편집자는 매 RFC 문서에 일련 번호를 부여한다. 일단 일련 번호를 부여 받고 출판되면, RFC는 절대 폐지되거나 수정되지 않는다. 만약 어떤 RFC 문서가 수정이 필요하다면, 저자는 수정된 문서를 다른 RFC 문서로 다시 출판해야 한다. 그러므로, 일부 RFC는 이전 버전의 RFC를 개선한 문서이며, 이전 버전의 RFC를 무효화하기도 한다. 이러한 덮어쓰는 방식을 통해, 번호 순으로 나열된 일련의 RFC는 인터넷 표준의 역사를 나타내기도 한다.

- **cron**:  소프트웨어 유틸리티로, 유닉스 계열 컴퓨터 운영 체제의 시간 기반 잡 스케줄러이다. 소프트웨어 환경을 설정하고 관리하는 사람들은 작업을 고정된 시간, 날짜, 간격에 주기적으로 실행할 수 있도록 스케줄링하기 위해 cron을 사용한다. ( [출처](https://ko.wikipedia.org/wiki/Cron) )

- **TCP 3-Way Handshake** [출처](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake): 
  - TCP/IP 프로토콜을 사용하는 응용프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.
  - **SYN**는 'Synchronize sequence numbers' 를 의미하고,
  - **ACK**는 'Acknowledgement'를 의미한다.
    ![image](https://user-images.githubusercontent.com/87057868/139587926-0f3e2632-88a2-4b94-9240-38d9f32ab6e0.png)
    [이미지 출처](https://mindnet.tistory.com/entry/네트워크-쉽게-이해하기-22편-TCP-3-WayHandshake-4-WayHandshake)


# References

- [출처-1](https://java119.tistory.com/52)
- [출처-2](https://java119.tistory.com/24)
- [출처-3](https://ko.wikipedia.org/wiki/%ED%98%91%EC%A0%95_%EC%84%B8%EA%B3%84%EC%8B%9C)
