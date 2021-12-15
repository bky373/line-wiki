# H2 데이터베이스를 사용한 스프링 부트

## 1. 개요

* 이번 문서에서는 스프링 부트 생태계에서 지원하는 H2 사용 방법에 대해 알아본다.



## 2. 종속성

* 먼저 H2 와 spring-boot-starter-data-jpa 종속성을 추가하자.
* Maven 방식
  ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
  ```
  


## 3. 데이터베이스 구성

* 기본적으로 스프링 부트는 사용자 이름은 sa와 빈 암호를 사용하여 메모리 내(in-memory) 저장소를 연결하도록 애플리케이션을 구성한다.
* 다음 속성을 application.properties 파일에 추가하면 필요에 따라 매개변수들을 변경할 수 있다.
  ```properties
  spring.datasource.url=jdbc:h2:mem:testdb
  spring.datasource.driverClassName=org.h2.Driver
  spring.datasource.username=sa
  spring.datasource.password=password
  spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
  ```
* 또는 아래와 같이 application.yaml 파일에서 애플리케이션 데이터베이스를 구성해도 좋다.
  ```yaml
    spring:
      datasource:
        url: jdbc:h2:mem:mydb
        username: sa
        password: password
        driverClassName: org.h2.Driver
      jpa:
        spring.jpa.database-platform: org.hibernate.dialect.H2Dialect
  ```
* 기본적으로 인메모리 데이터베이스는 휘발성이다. 따라서 애플리케이션을 다시 시작하면 데이터 손실이 발생한다.
* 파일 기반의 저장소를 사용하면 이러한 동작을 변경할 수 있다. 이를 위해 spring.datasource.url 속성을 업데이트해야 한다.
  ```yaml
    spring.datasource.url=jdbc:h2:file:/data/demo
  ```
* 마찬가지로 application.yaml에서 파일 기반 저장소에 대해 동일한 속성을 추가할 수 있다.
  ```yaml
    spring:
      datasource:
        url: jdbc:h2:file:/data/demo
  ```
* H2 데이터베이스는 [다양한 모드](http://www.h2database.com/html/features.html#connection_modes) 에서 동작 가능하다(Embedded mode, Server mode, Mixed mode 등).



## 4. 데이터베이스 작업

* Spring Boot 안에서 H2로 CRUD 작업을 수행하는 것은 다른 SQL 데이터베이스와 동일하다(자세한 사항은 [Spring Persistence Tutorial](https://www.baeldung.com/persistence-with-spring-series) 를 참고하자).

## 4.1. 데이터 소스 초기화

* 기본 SQL 스크립트를 사용하여 데이터베이스를 초기화할 수 있다. SQL 스크립트를 사용하기 위해 `src/main/resources` 디렉토리에 `data.sql` 파일을 추가하자.
* 여기에서는 일부 샘플 데이터로 countires 테이블을 채운다.
  ```sql
  INSERT INTO countries (id, name) VALUES (1, 'USA');
  INSERT INTO countries (id, name) VALUES (2, 'France');
  INSERT INTO countries (id, name) VALUES (3, 'Brazil');
  INSERT INTO countries (id, name) VALUES (4, 'Italy');
  INSERT INTO countries (id, name) VALUES (5, 'Canada');
  ```
* Spring Boot는 이 파일을 자동으로 선택하여 H2 인스턴스와 같은 임베디드 인메모리 데이터베이스를 실행한다. 테스트 또는 초기화 목적으로 데이터베이스를 채우기에 좋은 방법이다.
* 만약 spring.sql.init.mode 속성을 Never로 설정한다면 이 기본 동작을 비활성화할 수 있다. 또한 초기 데이터를 로드하도록 여러 SQL 파일을 구성할 수도 있다.
* [초기 데이터 로드](https://www.baeldung.com/spring-boot-data-sql-and-schema-sql) 와 관련해서는 여기를 참고하자.

## 4.2. Hibernate 와 data.sql

* 기본적으로 data.sql 스크립트는 Hibernate를 초기화 하기 전에 실행된다. 이는 스크립트에 기반한 초기화를 다른 데이터베이스 마이그레이션 툴(Flyway, Liquibase 등)과 일치시키기 위. Hibernate에 의해 생성된 스키마를 매번 다시 생성할 때 추가 속성을 설정해야 합니다.










# [ 참고 자료 ]
* [출처](https://www.baeldung.com/spring-boot-h2-database)
