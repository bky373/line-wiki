# HikariCP 소개

## 1. 개요
* [HikariCP JDBC](https://github.com/brettwooldridge/HikariCP) Connection Pool 프로젝트에 대해 알아본다. 
* 이는 2012년 Brett Wooldridge가 개발한 매우 가볍고(약 130Kb) 빠른 JDBC 연결 풀링 프레임워크다.


## 2. 소개
* 아래는 HikariCP의 성능을 c3p0, dbcp2, tomcat 및 vibur와 같은 다른 연결 풀링 프레임워크와 비교하였을 때 확인할 수 있는 몇 가지 벤치마크 결과이다. 
* 이 결과는 HikariCP 팀에서 발표하였다(원본 결과는 [여기](https://github.com/brettwooldridge/HikariCP-benchmark) 에서 확인할 수 있다).
  ![image](https://user-images.githubusercontent.com/49539592/146905029-80b7a9fc-cf74-4e74-a06c-40a8e35ac1ee.png)
* 아래와 같은 기술들이 적용되었기 때문에 프레임워크는 매우 빠르게 동작한다.
  * **바이트코드 수준 엔지니어링(Bytecode-level engineering)**: 어셈블리 수준(assembly level)의 네이티브 코딩을 포함하여 몇 가지 바이트코드 수준의 엔지니어링 작업을 완료하였다.
  * **마이크로 최적화(Micro-optimizations)**: 거의 측정할 수 없긴 하지만 마이크로한 최적화를 결합하면 전체 성능이 향상된다.
  * **Collections 프레임워크의 지능적인 사용**: `ArrayList<Statement>` 를 커스텀 클래스인 `Fast<List>` 로 대체하였다. 해당 클래스는 범위 검사(range checking)를 제거하고 시작부터 끝까지 제거(removal) 스캔을 수행한다.


## 3. Maven / Gradle 종속성
* HikariCP는 JVM의 모든 주요 버전을 지원한다. 
* 먼저 샘플 애플리케이션을 빌드하기 위해 다음 종속성을 추가하자(자바 8부터 11까지 커버한다).
  ```xml
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>3.4.5</version>
    </dependency>
  ```
* 물론 이전 JDK 버전(6, 7 등)을 위한 종속성도 있다. 각 버전에 맞게 [여기](https://search.maven.org/classic/#search%7Cgav%7C1%7Cg%3A%22com.zaxxer%22%20AND%20a%3A%22HikariCP-java6%22) 와 [여기](https://search.maven.org/classic/#search%7Cgav%7C1%7Cg%3A%22com.zaxxer%22%20AND%20a%3A%22HikariCP-java7%22) 를 참고해 종속성을 추가하자. 
* [Central Maven Repository](https://search.maven.org/classic/#search%7Cgav%7C1%7Cg%3A%22com.zaxxer%22%20AND%20a%3A%22HikariCP%22) 에서는 최신 버전을 확인할 수 있다.
* Gradle 을 사용할 경우 `build.gradle` 파일에 다음 종속성을 추가하자.
  ```
  dependencies {
    ...
  
    // https://mvnrepository.com/artifact/com.zaxxer/HikariCP
    implementation 'com.zaxxer:HikariCP:3.4.5'
  
    ...
  }
  ```


## 4. 사용하기
* 이제 샘플 애플리케이션을 만들자. 먼저 `pom.xml` 에 H2 등의 JDBC 드라이버 클래스 종속성을 추가해야 한다(Gradle 빌드는 `build.gradle` 에 추가한다). 
* 종속성이 없을 경우 애플리케이션에서 ClassNotFoundException 이 발생한다.
> 섹션 4 에서는 단순 설명을 위해 템플릿 코드를 작성하였다. 정상적으로 작동하는 코드를 보려면 섹션 5를 참고하자.

### 4.1. DataSource 생성
* HikariCP의 DataSource를 사용하여, 애플리케이션을 위한 데이터소스 단일 인스턴스를 생성한다.
  ```java
    public class DataSource {

      private static HikariConfig config = new HikariConfig();
      private static HikariDataSource hikariDataSource;
  
      static {
          config.setJdbcUrl( "jdbc_url" );
          config.setUsername( "database_username" );
          config.setPassword( "database_password" );
          config.addDataSourceProperty( "cachePrepStmts" , "true" );
          config.addDataSourceProperty( "prepStmtCacheSize" , "250" );
          config.addDataSourceProperty( "prepStmtCacheSqlLimit" , "2048" );
          hikariDataSource = new HikariDataSource( config );
      }
  
      private DataSource() {}
  
      public static Connection getConnection() throws SQLException {
          return hikariDataSource.getConnection();
      }
  }
  ```
  * 여기서 static 블록 초기화를 주목 하자. [HikariConfig](https://github.com/openbouquet/HikariCP/blob/master/src/main/java/com/zaxxer/hikari/HikariConfig.java) 는 데이터소스를 초기화하는 데 사용되는 구성 클래스다. 
  * HikariConfig에서는 사용자 이름(`username`), 암호(`password`), jdbcUrl 및 dataSourceClassName 라는 4가지 필수 매개변수를 제공한다.
  * 일반적으로 jdbcUrl 과 dataSourceClassName 둘 중 하나를 사용한다. 그러나 오래된 드라이버를 사용할 때는 두 가지 속성 설정이 모두 필요할 수도 있다.
  * 이러한 속성 외에도 다른 풀링 프레임워크에는 없는 다른 속성을 사용할 수 있다.
    * autoCommit
    * connectionTimeout
    * idleTimeout
    * maxLifetime
    * connectionTestQuery
    * connectionInitSql
    * validationTimeout
    * maximumPoolSize
    * poolName
    * allowPoolSuspension
    * readOnly
    * transactionIsolation
    * leakDetectionThreshold
  * 위의 속성에 대한 자세한 설명은 [여기](https://github.com/brettwooldridge/HikariCP) 에서 찾을 수 있다.
  * HikariCP는 이제 자체적으로 연결 누수(connection leaks)를 감지할 수 있을 정도로 많이 발전하였다.
  * 참고로, 리소스(`resources`) 디렉토리에 있는 속성 파일을 사용하여 HikariConfig 를 초기화할 수도 있다.
  * 이를 위해 다음 자바 코드와 속성 파일 내용을 작성하여 사용하자.
    ```java
      private static HikariConfig config = new HikariConfig("datasource.properties" );
    ```
    ```properties
      dataSourceClassName= //TBD
      dataSource.user= //TBD
      // 다른 속성 이름도 dataSource 접두사로 시작해야 한다. 
    ```
  * `//TBD` 자리에는 원하는 값을 대신 넣어주면 된다(TBD = To Be Determined or Decided, `미정` 이라는 뜻이다).
  * 또한 `java.util.Properties` 기반의 구성을 사용할 수도 있다.
    ```java
      Properties props = new Properties();
      props.setProperty( "dataSourceClassName" , //TBD );
      props.setProperty( "dataSource.user" , //TBD );
      //setter for other required properties
      private static HikariConfig config = new HikariConfig( props );
    ```
  * 마지막으로 처음 자바 코드를 살짝 수정하여 데이터소스를 직접 초기화할 수도 있다.
    ```java
      hikariDataSource.setJdbcUrl( //TBD  );
      hikariDataSource.setUsername( //TBD );
      hikariDataSource.setPassword( //TBD );
    ```

### 4.2. 데이터소스 사용
* 앞에서 데이터소스를 정의하였기 때문에, 이제 구성된 연결 풀(configured connection pool)에서 **연결(connection) 객체를 얻을 수 있다.** 그리고 **JDBC 와 관련한 작업을 수행할 수 있다.**
* 예시로 두 개의 테이블: 부서(`dept`) 와 직원(`emp`) 이 있다고 하자. 
* 앞서 정의한 HikariCP를 사용하여, 결과적으로 데이터베이스로부터 세부 정보를 가져오는 클래스를 작성할 것이다.
* 먼저 샘플 데이터를 생성하기 위해 필요한 SQL 문을 작성하자.
  ```sql
    // db.sql
  
    create table dept(
      deptno numeric,
      dname  varchar(14),
      loc    varchar(13),
      constraint pk_dept primary key ( deptno )
    );
    
    create table emp(
      empno    numeric,
      ename    varchar(10),
      job      varchar(9),
      mgr      numeric,
      hiredate date,
      sal      numeric,
      comm     numeric,
      deptno   numeric,
      constraint pk_emp primary key ( empno ),
      constraint fk_deptno foreign key ( deptno ) references dept ( deptno )
    );
    
    insert into dept values( 10, 'ACCOUNTING', 'NEW YORK' );
    insert into dept values( 20, 'RESEARCH', 'DALLAS' );
    insert into dept values( 30, 'SALES', 'CHICAGO' );
    insert into dept values( 40, 'OPERATIONS', 'BOSTON' );
    
    insert into emp values(
      7839, 'KING', 'PRESIDENT', null,
      to_date( '17-11-1981' , 'dd-mm-yyyy' ),
      7698, null, 10
    );
    insert into emp values(
      7698, 'BLAKE', 'MANAGER', 7839,
      to_date( '1-5-1981' , 'dd-mm-yyyy' ),
      7782, null, 20
    );
    insert into emp values(
      7782, 'CLARK', 'MANAGER', 7839,
      to_date( '9-6-1981' , 'dd-mm-yyyy' ),
      7566, null, 30
    );
    insert into emp values(
      7566, 'JONES', 'MANAGER', 7839,
      to_date( '2-4-1981' , 'dd-mm-yyyy' ),
      7839, null, 40
    );
  ```
* H2와 같은 인메모리 데이터베이스를 사용하는 경우, 코드를 실행하여 데이터를 가져오기 전에 데이터베이스 스크립트를 자동으로 로드해야 한다. 
* 다행히 H2에는, 런타임에 클래스 경로(`classpath`)에서 데이터베이스 스크립트를 로드할 수 있는 INIT 매개변수가 있다. 이를 사용하면 JDBC URL을 다음과 같이 설정해야 한다.
  ```java
    jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;INIT=runscript from 'classpath:/db.sql'
  ```
* 그런 다음, 데이터베이스에서 데이터를 가져오는 메서드를 만들어야 한다.
  ```java
    public static List<Employee> fetchData() throws SQLException {
      String SQL_QUERY = "select * from emp";
      List<Employee> employees = null;
  
      try (Connection con = DataSource.getConnection();
          PreparedStatement pst = con.prepareStatement( SQL_QUERY );
          ResultSet rs = pst.executeQuery();) {
              
              employees = new ArrayList<>();
              Employee employee;
              while ( rs.next() ) {
                  employee = new Employee();
                  employee.setEmpNo( rs.getInt( "empno" ) );
                  employee.setEname( rs.getString( "ename" ) );
                  employee.setJob( rs.getString( "job" ) );
                  employee.setMgr( rs.getInt( "mgr" ) );
                  employee.setHiredate( rs.getDate( "hiredate" ) );
                  employee.setSal( rs.getInt( "sal" ) );
                  employee.setComm( rs.getInt( "comm" ) );
                  employee.setDeptno( rs.getInt( "deptno" ) );
                  employees.add( employee );
              }
      } 
      return employees;
  }
  ```
* 마지막으로 JUnit 테스트를 만들어 메서드를 테스트하자. `emp` 테이블의 행 수를 알고 있으므로 반환된 목록의 크기가 행 수와 같아야 한다.
  ```java
    @Test
    public void givenConnection_thenFetchDbData() throws SQLException {
        HikariCPDemo.fetchData();
    
        assertEquals( 4, employees.size() );
    }
  ```

## 참고 자료
* [Introduction to HikariCP](https://www.baeldung.com/hikaricp)
