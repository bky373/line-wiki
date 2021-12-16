# Hibernate Getting Started Guide

> 이번 가이드에서 사용하는 실습 코드는 [이곳 Preface 하단](https://docs.jboss.org/hibernate/orm/5.6/quickstart/html_single/#preface) 에서 다운로드할 수 있다.

## 머리말

* 사용하고자 하는 데이터의 개체(Objects) 표현 방식과 관계형 데이터베이스(Relational databases)의 표현 방식이 일치하지 않을 경우 개발 시간 및 비용이 많이 소모된다.
* Hibernate 는 위의 문제를 해결할 수 있도록 개발된 **Java 환경에서 사용하는 ORM(Object/Relational Mapping) 솔루션** 이다.
  > 개체/관계형 매핑(Object/Relational Mapping) 이라는 용어는 개체 모델 표현과 관계형 데이터 모델 표현 간의 데이터를 매핑하는 기술을 의미한다.
* Hibernate 는 Java 클래스에서 데이터베이스 테이블로, Java 데이터 유형에서 SQL 데이터 유형으로 매핑을 처리한다.
* 또한 데이터 쿼리 및 검색 기능을 제공하기 때문에 SQL 및 JDBC에서 수동 데이터 처리 시간을 크게 단축시킨다.
* Hibernate의 설계 목표는 개발자의 수동적인 데이터 영속성 관련 프로그래밍 작업을 95% 덜어주는 것에 있다.

## 1. Hibernate 설치 및 기본 설명
### 1.1. Hibernate 모듈/아티팩트(Modules/Artifacts)
* Hibernate의 기능은 의존성(모듈성)을 분리하기 위해 여러 개의 모듈/아티팩트로 분할된다.  

* **hibernate-core**
    * 메인(코어) Hibernate 모듈이다.
    * ORM 기능들과 여러 API, 다양한 통합 SPI 를 정의한다.

* **hibernate-envers**
    * Hibernate의 과거 엔티티 버전 관리 기능 을 포함한다. 

* **hibernate-spatial**
    * Hibernate의 공간/GIS 데이터 유형 을 지원한다.

* **hibernate-osgi**
    * OSGi 컨테이너에서 실행하기 위한 Hibernate 지원 기능을 포함한다.

* **hibernate-agroal**
    * Agroal 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.

* **hibernate-c3p0**
    * C3P0 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
    
* **hibernate-hikaricp**
    * HikariCP 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
        
* **hibernate-vibur**
    * Vibur DBCP 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
     
* **hibernate-proxool**
    * Proxool 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
     
* **hibernate-jcache**
    * Ehcache 캐싱 스펙(specification)을 Hibernate에 통합하여 호환되는 모든 구현이 second-level 캐시 공급자가 되도록 한다.
     
* **hibernate-ehcache**
    * Ehcache 캐싱 라이브러리를 second-level 캐시 공급자로 Hibernate에 통합한다.


### 1.2. Release Bundle 다운로드
* [이곳](https://sourceforge.net/projects/hibernate/files/hibernate-orm/) 에서 릴리스 번들을 다운로드할 수 있다.
* 각각의 릴리스 번들은 JAR 파일, 문서, 소스 코드 등을 포함하고 있으며, 보다 자세한 구성은 아래와 같다.
    * `lib/required/` 디렉토리는 `hibernate-core` jar 및 이와 관련된 모든 종속성을 포함한다. 이곳의 jar 들은 Hibernate의 어떤 기능이 사용되든 상관없이 클래스 경로(classpath)에서 사용 가능해야 한다.
    * `lib/envers` 디렉토리는 `hibernate-envers` jar 및 이와 관련된 모든 종속성(`lib/required/` 및 `lib/jpa/`에 있는 것 이상)을 포함한다.
    * `lib/spatial/` 디렉토리는 `hibernate-spatial` jar 및 이와 관련된 모든 종속성(`lib/required/`에 있는 것 이상)을 포함한다.
    * `lib/osgi/` 디렉토리는 `hibernate-osgi` jar 및 이와 관련된 모든 종속성(`lib/required/` 및 `lib/jpa/`에 있는 것 이상)을 포함한다.
    * `lib/jpa-metamodel-generator/` 디렉토리는 Criteria API 타입 안전(type-safe) 메타모델을 생성하는 데 필요한 jar를 포함한다.
    * `lib/optional/` 디렉토리는 Hibernate가 제공하는 다양한 연결 풀링(connection pooling)과 second-level 캐시 통합에 필요한 jar 및 종속 항목을 포함한다.


### 1.3. Maven 저장소(Repository) 아티팩트

* Hibernate 아티팩트에 관하여 권한을 가지고 있는 저장소는 JBoss Maven 저장소다. 
* Hibernate 아티팩트는 자동화된 작업을 거쳐 Maven Central에 동기화된다(약간의 지연이 발생할 수 있다).
* Hibernate ORM 아티팩트는 `org.hibernate` groupId 아래 게시된다.



## 2. 네이티브 Hibernate API와 hbm.xml 매핑을 사용한 튜토리얼

> 이 튜토리얼은 다운로드 번들의 `basic/` 아래에 있다.

* 이번 섹션의 목표는 다음과 같다.
  * Hibernate SessionFactory 를 부트스트랩(로드) 한다.
  * Hibernate 매핑(hbm.xml) 파일을 사용하여 매핑 정보를 제공한다.
  * Hibernate Native API 를 사용한다.

### 2.1. Hibernate 구성(configuration) 파일

* 이 튜토리얼에서 `hibernate.cfg.xml` 파일은 Hibernate 구성 정보를 정의한다.
* 아래 `<property/>` 요소들은 JDBC 연결 정보를 정의한다.
  * `connection.driver_class`
  * `connection.url`
  * `connection.username`
  * `connection.password`
* 튜토리얼에서는 H2 인메모리 데이터베이스를 활용하기 때문에 이러한 속성들의 값은 모두 인메모리 모드에서 H2를 실행하는 것과 관련 있다.
* `connection.pool_size`는 Hibernate의 내장 연결 풀(built-in connection pool)에서 연결 개수(the number of connections)를 설정하는 데 사용된다.
* `dialect` 속성은 Hibernate가 대화할 특정 SQL variant(SQL의 변형 중 하나)를 지정한다.
  > 대부분의 경우 Hibernate는 dialect 를 적절하게 선별할 수 있다. 이는 애플리케이션이 여러 데이터베이스를 대상으로 하는 경우에 특히 유용하다.
* `hbm2ddl.auto` 속성을 사용하면 데이터베이스 스키마를 데이터베이스에 자동 생성할 수 있다.
* 마지막으로 영속 클래스(persistent classes)를 위한 매핑 파일을 구성에 추가한다. `<mapping/>` 요소의 `resource` 속성은 Hibernate가 클래스 경로 자원(classpath resource)으로 매핑을 위치시키도록 한다(이때 `java.lang.ClassLoader` 조회를 사용한다).
* Hibernate SessionFactory를 부트스트랩하는 방법은 여러 가지가 있다. 자세한 내용은 Native Bootsgtrapping topical guide 를 참조하자.

### 2.2. 엔티티 자바 클래스

* 이 튜토리얼에서 사용할 엔티티 클래스는 `org.hibernate.tutorial.hbm.Event` 이다.
* 문서 최상단에서 소스 코드를 다운 받은 다음 확인하자.


### 2.3. 매핑 파일

* 매핑 파일은 클래스 경로 리소스 `org/hibernate/tutorial/hbm/Event.hbm.xml` 이다.
* Hibernate는 매핑 메타데이터를 사용하여 영속 클래스의 객체를 로드하고 저장하는 방법을 결정한다. 
* Hibernate 매핑 파일은 이 메타데이터를 Hibernate에 제공하기 위한 방법 중 하나다.
  * 예제 1. 클래스 매핑 요소
    ```xml
    <class name="Event" table="EVENTS">
        ...
    </class>
    ```
    * `name` 속성은 엔티티로 정의될 클래스의 [FQN](https://zetawiki.com/wiki/%EC%99%84%EC%A0%84%EC%88%98%EC%8B%9D%EB%AA%85_FQN) 을 지정한다. 
    * `table` 속성은 매핑 엔티티에 대한 데이터를 포함하는 데이터베이스 테이블의 이름을 지정한다.
    * **이제 Event 클래스의 인스턴스가 EVENTS 데이터베이스 테이블의 행에 매핑된다.**
  * 예제 2. id 매핑 요소
    ```xml
    <id name="id" column="EVENT_ID">
        <generator class="increment"/>
    </id>
    ```
    * Hibernate는 테이블의 행을 고유하게 식별하기 위해 `<id/>` 요소에 의해 명명된 속성을 사용한다.
    * 여기서 `<id/>` 요소는 `EVENT_ID` 열의 이름을 `EVENTS` 테이블의 기본 키로 지정한다. 또한 `Event` 클래스의 `id` 속성을 식별자 값을 포함하는 속성으로 식별한다.
    * `generator` 요소는 엔티티의 기본 키(primary key) 값을 생성하는 데 사용되는 전략을 Hibernate에 알리는 역할을 한다. 이 예에서는 단순 증가 카운트를 사용한다.
  * 예제 3. 속성(property) 매핑 요소
    ```xml
    <property name="date" type="timestamp" column="EVENT_DATE"/>
    <property name="title"/>
    ```
    * 두 개의 `<property/>` 요소는 `Event` 클래스의 나머지 속성인 날짜(`date`)와 제목(`title`)을 선언한다. 
    * 날짜(`date`) 속성 매핑에는 마지막에 `column` 속성이 포함되어 있지만 제목(`title`)에는 포함되어 있지 않다. `column` 속성이 없으면 Hibernate는 속성 이름을 `column` 이름으로 사용한다. 대부분의 데이터베이스에서 `date`는 예약어이기 때문에 `column` 이름에 예약어가 아닌 단어를 지정해야 한다.
    * 제목(`title`) 매핑에는 `type` 속성도 없다. 매핑 파일에서 선언되고 사용되는 타입은 Java 데이터 타입이나 SQL 데이터베이스 타입이 아니다. 대신 Java와 SQL 데이터 타입의 변환기 역할을 하는 Hibernate의 매핑 유형이다.
    * Hibernate는 `type` 속성이 지정된 경우나 그렇지 않은 경우 모두 올바른 타입 변환을 시도한다. 특히 `type` 속성이 매핑에 지정되어 있지 않으면, 선언된 속성의 Java 타입을 결정하기 위해 Java 리플렉션을 사용하고 해당 Java 타입의 기본(default) 매핑 유형을 사용하여 자동으로 매핑 유형을 결정한다.
    * 경우에 따라 이 자동 감지는 날짜(`date`) 속성에서 알 수 있는 것처럼, 개발자가 의도한 기본값을 선택하지 않을 수 있다. Hibernate는 `java.util.Date` 라는 자바 타입이 SQL DATE, TIME 또는 TIMESTAMP 중 어느 데이터 유형에 매핑되어야 하는지 여부를 알 수 없다. 
    * 전체 날짜 및 시간(Full date and time) 정보는 해당하는 속성을 `timestamp` 변환기(converter)에 매핑하여 보존한다(이 변환기는 `org.hibernate.type.TimestampType` 변환기 클래스를 식별한다).
    > Hibernate는 매핑 파일이 처리될 때 리플렉션을 사용하여 매핑 유형을 결정한다. 이 프로세스는 시간과 리소스 측면에서 오버헤드를 더한다. 시작 시 성능이 중요한 경우 사용할 유형을 명시적으로 정의하자.


### 2.4. 예제 테스트 코드

* `org.hibernate.tutorial.hbm.NativeApiIllustrationTest` 클래스는 Hibernate 네이티브 API를 사용하는 것을 보여준다.
  > 튜토리얼 예제에서는 편의성을 위해 JUnit 테스트를 사용한다. <br> 
  > setUp 호출에서 애플리케이션 시작 시 `org.hibernate.SessionFactory`가 생성되고 tearDown 호출에서 애플리케이션 라이프사이클이 끝날 때 `org.hibernate.SessionFactory`이 닫히는 것을 볼 수 있다.

* 예제 4. `org.hibernate.SessionFactory` 얻기
  ```java
    protected void setUp() throws Exception {
      // A SessionFactory is set up once for an application!
      final StandardServiceRegistry registry = new StandardServiceRegistryBuilder()
              .configure() // configures settings from hibernate.cfg.xml
              .build();
      try {
          sessionFactory = new MetadataSources( registry ).buildMetadata().buildSessionFactory();
      }
      catch (Exception e) {
          // The registry would be destroyed by the SessionFactory, but we had trouble building the SessionFactory
          // so destroy it manually.
          StandardServiceRegistryBuilder.destroy( registry );
      }
    }
  ```
  * setUp 메소드는 먼저 SessionFactory가 사용할 서비스 세트에 구성 정보를 통합하는 `org.hibernate.boot.registry.StandardServiceRegistry` 인스턴스를 빌드한다. 이 튜토리얼에서는 `hibernate.cfg.xml`에 모든 구성 정보를 미리 정의해두었다.
  * StandardServiceRegistry를 사용하여 `org.hibernate.boot.MetadataSources` 를 생성하는데, 이는  도메인 모델에 대해 Hibernate에 알리는 시작점이다. 자세한 구성 정보는 `hibernate.cfg.xml`를 참고하자.
  * `org.hibernate.boot.Metadata` 는 SessionFactory가 기반으로 사용할 애플리케이션 도메인 모델의 뷰(view)를 나타낸다.
  * 부트스트랩 프로세스의 마지막 단계는 SessionFactory를 빌드하는 것이다. SessionFactory는 애플리케이션 전체에서 사용되는 스레드 안전한 개체(object)이다(인스턴스화가 한 번 이루어진다).
  * SessionFactory는 `org.hibernate.Session` 인스턴스를 위한 팩토리 역할을 한다. 세션은 하나의 "작업 단위(a unit of work)" 결로 생각하고 사용해야 한다.

* 예제 5. 엔티티 저장하기
  ```java
  public void testBasicUsage() {
    Session session = sessionFactory.openSession();
    session.beginTransaction();
    session.save( new Event( "Our very first event!", new Date() ) );
    session.save( new Event( "A follow up event", new Date() ) );
    session.getTransaction().commit();
    session.close();
    ...
  }
  ```
  * `testBasicUsage()`는 먼저 몇 가지 새로운 Event 객체를 생성하고 save() 메소드를 사용하여 Hibernate에 넘겨준다. 
  * Hibernate는 이제 각 Event에 대해 데이터베이스에서 INSERT를 수행하는 책임을 진다.
  
* 예제 6. 엔터티 목록 가져오기
  ```java
  public void testBasicUsage() {
    ...
    session = sessionFactory.openSession();
    session.beginTransaction();
    List result = session.createQuery( "from Event" ).list();
    for ( Event event : (List<Event>) result ) {
    System.out.println( "Event (" + event.getDate() + ") : " + event.getTitle() );
    }
    session.getTransaction().commit();
    session.close();
  }
  ```
  * 여기에서는 먼저 데이터베이스에 존재하는 모든 Event 객체를 로드하기 위한 적절한 SELECT SQL을 생성한다. 
  * 그리고 이를 데이터베이스로 보내어 Event 객체를 결과 세트(result set) 데이터로 받는다. 이때 결과 세트 데이터를 받기 위해 Hibernate Query Language(HQL)를 사용한 것을 볼 수 있다.
  * 결국 `testBasicUsage()` 테스트 실행 결과는 아래와 같다. 데이터베이스에 삽입한 이벤트 두 개가 의도한 대로 잘 출력되는 것을 볼 수 있다.
    <img width="1642" alt="image" src="https://user-images.githubusercontent.com/49539592/146234189-11172a4c-8f32-436b-97cb-454c190f9ab5.png">


## 3. 네이티브 Hibernate API 및 어노테이션 매핑을 사용하는 튜토리

> 이 튜토리얼은 다운로드한 번들의 `annotations/` 아래에 있다.

* 이번 섹션의 목표는 다음과 같다.
  * Hibernate SessionFactory 를 부트스트랩(로드) 한다.
  * 어노테이션을 사용하여 매핑 정보를 제공한다.
  * Hibernate Native API 를 사용한다.

### 3.1. Hibernate 구성 파일

* 구성파일의 내용은 아래 한 가지 차이점을 제외하고 [이곳](https://docs.jboss.org/hibernate/orm/5.6/quickstart/html_single/#hibernate-gsg-tutorial-basic-config) 의 내용과 동일하다.
* 차이점: `class` 속성을 사용하여 엔티티 클래스의 이름을 지정하는 맨 끝에 있는 `<mapping/>` 요소


### 3.2. 어노테이션이 달린 엔티티 Java 클래스

* 이 튜토리얼에서 엔티티 클래스는 JavaBean 규칙을 따르는 `org.hibernate.tutorial.annotations.Event` 다.
* 클래스 내용 자체는 위에서 사용한 엔티티 자바 클래스와 동일하고, 메타 데이터를 제공하는 방식만 달라졌다. 이번에는 별도의 매핑 파일을 사용하지 않고 어노테이션을 사용하여 메타데이터를 제공한다.

* 예제 7. 클래스를 엔터티로 식별하기
  ```java
    @Entity
    @Table( name = "EVENTS" )
    public class Event {
      private Long id;
    
      private String title;
      private Date date;
  
      public Event() {
        // this form used by Hibernate
      }
    
      public Event(String title, Date date) {
        // for application use, to create new events
        this.title = title;
        this.date = date;
      }
    }
  ```
  * `@javax.persistence.Entity` 어노테이션은 클래스를 엔터티로 표시하는 데 사용된다(이 기능은 [매핑 파일](https://docs.jboss.org/hibernate/orm/5.6/quickstart/html_single/#hibernate-gsg-tutorial-basic-mapping) 에서 설명한 `<class/>` 매핑 요소와 동일한 기능이다). 
  * 또한 `@javax.persistence.Table` 어노테이션은 테이블 이름을 명시적으로 지정한다. 이 사양이 없으면 기본 테이블 이름은 `EVENT` 가 된다.

* 예제 8. 식별자 속성을 식별하기
  ```java
    @Id
    @GeneratedValue(generator="increment")
    @GenericGenerator(name="increment", strategy = "increment")
    public Long getId() {
      return id;
    }
  ```
  * `@javax.persistence.Id` 는 엔티티의 식별자를 정의하는 속성을 표시한다.
  * `@javax.persistence.GeneratedValue` 와 `@org.hibernate.annotations.GenericGenerator` 는 Hibernate가 이 엔티티의 식별자 값에 대해 Hibernate의 `increment` 생성 전략을 사용해야 함을 나타내기 위해 함께 작동한다.
  
* 예제 9. 기본 속성(properties) 식별하기
  ```java
    public String getTitle() {
        return title;
    }
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "EVENT_DATE")
    public Date getDate() {
    return date;
    }
  ```
  * 매핑 파일에서와 같이 날짜(`date`) 속성은 특별한 이름 지정(예약어 사용 방지)과 SQL 유형을 설명하기 위해 특별한 처리가 필요하다.
  * 어노테이션으로 매핑할 때 엔티티 클래스의 속성은 기본적으로 영속적인 것으로 간주된다. 따라서 Event 엔티티에서 제목(`title`)과 관련된 매핑 정보를 따로 표시하지 않는다.
  

### 3.4 예제 테스트 코드

* `org.hibernate.tutorial.annotations.AnnotationsIllustrationTest` 는 기본적으로 앞의 [2.4. 예제 테스트 코드](https://docs.jboss.org/hibernate/orm/5.6/quickstart/html_single/#hibernate-gsg-tutorial-basic-test) 에서 논의된 `org.hibernate.tutorial.hbm.NativeApiIllustrationTest` 와 동일하다.



## 참고 자료
* [출처](https://docs.jboss.org/hibernate/orm/5.6/quickstart/html_single/)
