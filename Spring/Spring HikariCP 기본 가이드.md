# HikariCP 기본 가이드

## 1. 개요
* [HikariCP JDBC](https://github.com/brettwooldridge/HikariCP) Connection Pool 프로젝트에 대해 알아본다. 
* 이는 2012년 Brett Wooldridge가 개발한 매우 가볍고(약 130Kb) 빠른 JDBC 연결 풀링 프레임워크다.


## 2. 소개
* HikariCP 팀은, HikariCP의 성능을 다른 연결 풀링 프레임워크(c3p0, dbcp2, tomcat 및 vibur)와 비교하였을 때 확인할 수 있는 몇 가지 유의미한 결과를 발표하였다.
  ![image](https://user-images.githubusercontent.com/49539592/146905029-80b7a9fc-cf74-4e74-a06c-40a8e35ac1ee.png)
  > 원본 결과는 [여기](https://github.com/brettwooldridge/HikariCP-benchmark) 에서 확인할 수 있다.
* ConnectionCycle은 `DataSource.getConnection()/Connection.close()`의 주기(Cycle)를 의미한다. 
* StatementCycle은 `Connection.prepareStatement()`, `Statement.execute()`, `Statement.close()`의 주기(Cycle)를 의미한다.
* HikariCP는 아래 기술들이 적용되어 뛰어난 좋은 성능을 보여준다.
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
* 종속성이 없을 경우 애플리케이션은 ClassNotFoundException 을 던진다.
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
  * [HikariConfig](https://github.com/openbouquet/HikariCP/blob/master/src/main/java/com/zaxxer/hikari/HikariConfig.java) 는 데이터소스를 초기화하는 데 사용되는 구성 클래스다.
  * HikariConfig 에서는 사용자 이름(`username`), 암호(`password`), `jdbcUrl` 및 `dataSourceClassName` 라는 4가지 필수 매개변수를 사용한다. 여기에서는 static 블록에서 `dataSourceClassName`를 제외한 나머지 3가지 필수 매개변수를 초기화하였다.
  * 일반적으로 `jdbcUrl` 과 `dataSourceClassName`는 둘 중 하나만 사용한다. 하지만 오래된 드라이버를 사용할 때는 두 가지 설정이 모두 필요할 수 있다.
  * 위의 속성 외에도 다른 풀링(Pooling) 프레임워크에는 없는 다른 속성들을 사용할 수 있다. 자세히 알고 싶다면 [여기](https://github.com/brettwooldridge/HikariCP) 를 참고하자.
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
  * HikariConfig를 초기화하기 위해 리소스(`resources`) 디렉토리에 있는 속성 파일을 사용할 수도 있다(위에서는 자바 코드만 사용하였다).
  * 이를 위해 다음 자바 코드와 속성 파일을 사용하자.
    ```java
      private static HikariConfig config = new HikariConfig("datasource.properties" ); // 생성자 인수로 속성 파일 경로를 전달한다
    ```
    ```properties
      dataSourceClassName= //TBD
      dataSource.user= //TBD
      // 다른 속성 이름도 dataSource 접두사로 시작해야 한다. 
    ```
  * `//TBD` 자리에는 자신이 원하는 값을 넣어준다(TBD = To Be Determined or Decided, `미정` 이라는 뜻이다).
  * 한편, `java.util.Properties`를 기반으로 하여 구성할 수도 있다.
    ```java
      Properties props = new Properties();
      props.setProperty( "dataSourceClassName" , //TBD );
      props.setProperty( "dataSource.user" , //TBD );
      //setter for other required properties
      private static HikariConfig config = new HikariConfig( props );
    ```
  * 마지막으로 처음 자바 코드를 살짝 수정하여 데이터소스를 직접 초기화할 수도 있다.
    ```java
      hikariDataSource.setJdbcUrl( //TBD  ); // 앞에서는 config.setJdbcUrl(~) 를 사용하였다
      hikariDataSource.setUsername( //TBD );
      hikariDataSource.setPassword( //TBD );
    ```

### 4.2. 데이터소스 사용
* 앞에서 데이터소스를 정의하였기 때문에, 이제 구성된 연결 풀(configured connection pool)에서 **연결(connection) 객체** 를 얻을 수 있다. 그리고 **JDBC 관련한 작업** 을 수행할 수 있다.
* 예시로 두 개의 테이블: 부서(`dept`) 와 직원(`emp`) 이 있다고 하자. 
* 앞서 정의한 HikariCP를 사용하여, 결과적으로 데이터베이스로부터 세부 정보를 가져오는 메서드를 작성할 것이다.
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
* 인메모리 데이터베이스(예를 들어 H2)를 사용하는 경우, 코드를 실행하여 데이터를 가져오기 전에 데이터베이스 스크립트를 자동으로 로드해야 한다. 
* 다행히 H2에는, 런타임에 클래스 경로(`classpath`)에서 데이터베이스 스크립트를 로드할 수 있는 INIT 매개변수가 있다. 다음 내용을 참고하자.
  ```java
    jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;INIT=runscript from 'classpath:/db.sql'
  ```
* 그 다음, 데이터베이스에서 데이터를 가져오는 메서드를 만든다.
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
* 마지막으로 JUnit 테스트로 메서드를 테스트한다. `emp` 테이블의 행 수를 알고 있으므로 반환된 목록의 크기가 행 수와 같아야 한다.
  ```java
    @Test
    public void givenConnection_thenFetchDbData() throws SQLException {
        HikariCPDemo.fetchData();
    
        assertEquals( 4, employees.size() );
    }
  ```


## 5. 실행 코드
* 위에서 템플릿 코드를 봤으니 이제 실행 가능한 코드를 작성해보자. 실행 코드는 [여기](https://github.com/eugenp/tutorials/tree/master/libraries-data-db) 를 참고하였다.
* 프로젝트 구조는 다음과 같다(`playground.forspring` 이라는 패키지 이름은 전혀 중요하지 않다).

  <img width="248" alt="image" src="https://user-images.githubusercontent.com/49539592/146957785-f9cc32bd-3f7c-4662-b0a0-f5228bcc0ef9.png">

### 5.1. 드라이버 종속성 추가
* 예시에서는 Gradle 를 사용하였으므로, `build.gradle` 에 H2 드라이버 종속성을 추가한다.
  ```
    dependencies {
      ...
      implementation 'com.h2database:h2'
      ...
  }
  ```
* 종속성을 추가하지 않을 경우, 최종 코드 실행 후 다음 오류를 만난다.
  <img width="1666" alt="image" src="https://user-images.githubusercontent.com/49539592/146956885-f65f55c6-c208-4f88-ab47-dff6562852ff.png">

### 5.2. 데이터소스 구성
* 실행 가능한 코드로 데이터소스를 구성한다.
* **DataSource.java** (파일 위치는 위의 프로젝트 구조를 참고하자.)
  ```java
    public class DataSource {

      private static HikariConfig config = new HikariConfig();
      private static HikariDataSource hikariDataSource;
    
      static {
    
        config.setJdbcUrl("jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;INIT=runscript from 'classpath:/db.sql'");
        config.setUsername("");
        config.setPassword("");
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");
        hikariDataSource = new HikariDataSource(config);
    
        /*
         * 대안 1. 속성 파일(.properties) 기반의 구성 - datasource.properties 파일 작성 필수
         */
        // config = new HikariConfig("datasource.properties");
    
        /*
         * 대안 2. Properties 기반의 구성
         */
        // Properties props = new Properties();
        // props.setProperty("dataSourceClassName", "org.h2.Driver");
        // props.setProperty("dataSource.user", "");
        // props.setProperty("dataSource.password", "");
        // props.put("dataSource.logWriter", new PrintWriter(System.out));
        // config = new HikariConfig(props);
    
        /*
         * 대안 2. HikariConfig 를 직접 초기화
         */
        // hikariDataSource.setJdbcUrl("jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;INIT=runscript from 'classpath:/db.sql'");
        // hikariDataSource.setUsername("");
        // hikariDataSource.setPassword("");
      }
    
      private DataSource() {
      }
    
      public static Connection getConnection() throws SQLException {
        return hikariDataSource.getConnection();
      }
    
    }
  ```

### 5.3. 테이블 및 데이터 생성
* `db.sql` 에 `dept` 및 `emp` 테이블 생성 쿼리를 작성한다.
* 동일한 파일에 샘플 데이터 삽입 쿼리를 작성한다.
* **db.sql** (파일 위치는 위의 프로젝트 구조를 참고하자.)
  ```java
    drop table if exists emp;
    drop table if exists dept;
    
    
    create table dept(
      deptno numeric,
      dname  varchar(14),
      loc    varchar(13),
      constraint pk_dept primary key (deptno)
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
      constraint pk_emp primary key (empno),
      constraint fk_deptno foreign key (deptno) references dept (deptno)
    );
    
    insert into dept values(10, 'ACCOUNTING', 'NEW YORK');
    insert into dept values(20, 'RESEARCH', 'DALLAS');
    insert into dept values(30, 'SALES', 'CHICAGO');
    insert into dept values(40, 'OPERATIONS', 'BOSTON');
    
    insert into emp values(
      7839, 'KING', 'PRESIDENT', null,
      to_date('17-11-1981','dd-mm-yyyy'),
      7698, null, 10
    );
    insert into emp values(
      7698, 'BLAKE', 'MANAGER', 7839,
      to_date('1-5-1981','dd-mm-yyyy'),
      7782, null, 20
    );
    insert into emp values(
      7782, 'CLARK', 'MANAGER', 7839,
      to_date('9-6-1981','dd-mm-yyyy'),
      7566, null, 30
    );
    insert into emp values(
      7566, 'JONES', 'MANAGER', 7839,
      to_date('2-4-1981','dd-mm-yyyy'),
      7839, null, 40
    );
    
    commit;
  ```

### 5.4. 데이터소스 연결 및 데이터 확인
* 앞에서 데이터소스 구성 및 데이터 삽입을 마쳤기 때문에 이제 데이터베이스로부터 실제 데이터를 가져오는지 확인하는 작업이 남았다.
* HikariCPDemo 클래스 안에 `emp` 테이블의 모든 데이터를 가져오는 `fetchData()` 메서드를 정의하고, 이를 `main()` 메서드에서 호출하자.
* 그리고 실제로 어떤 결과를 출력하는지 눈으로 확인하자.
* **HikariCPDemo.java** (파일 위치는 위의 프로젝트 구조를 참고하자.)
  ```java
    public class HikariCPDemo {

      public static List<Employee> fetchData() {
        final String SQL_QUERY = "select * from emp";
        List<Employee> employees = null;
    
        try (Connection con = DataSource.getConnection();
             PreparedStatement pst = con.prepareStatement(SQL_QUERY);
             ResultSet rs = pst.executeQuery();) {
    
          employees = new ArrayList<Employee>();
          Employee employee;
          while (rs.next()) {
            employee = new Employee();
            employee.setEmpNo(rs.getInt("empno"));
            employee.setEname(rs.getString("ename"));
            employee.setJob(rs.getString("job"));
            employee.setMgr(rs.getInt("mgr"));
            employee.setHiredate(rs.getDate("hiredate"));
            employee.setSal(rs.getInt("sal"));
            employee.setComm(rs.getInt("comm"));
            employee.setDeptno(rs.getInt("deptno"));
            employees.add(employee);
          }
        } catch (SQLException e) {
          e.printStackTrace();
        }
        return employees;
      }
    
      public static void main(String[] args) {
        List<Employee> employees = fetchData();
        employees.forEach(System.out::println);
      }
    
    }
  ```
* 위에서 Employee 라는 타입을 사용하므로 Employee 클래스도 따로 정의해준다.
* **Employee.java** (파일 위치는 위의 프로젝트 구조를 참고하자.)
  ```java
    public class Employee {

      private int empNo;
      private String ename;
      private String job;
      private int mgr;
      private Date hiredate;
      private int sal;
      private int comm;
      private int deptno;
    
      public int getEmpNo() {
        return empNo;
      }
    
      public void setEmpNo(int empNo) {
        this.empNo = empNo;
      }
    
      public String getEname() {
        return ename;
      }
    
      public void setEname(String ename) {
        this.ename = ename;
      }
    
      public String getJob() {
        return job;
      }
    
      public void setJob(String job) {
        this.job = job;
      }
    
      public int getMgr() {
        return mgr;
      }
    
      public void setMgr(int mgr) {
        this.mgr = mgr;
      }
    
      public Date getHiredate() {
        return hiredate;
      }
    
      public void setHiredate(Date hiredate) {
        this.hiredate = hiredate;
      }
    
      public int getSal() {
        return sal;
      }
    
      public void setSal(int sal) {
        this.sal = sal;
      }
    
      public int getComm() {
        return comm;
      }
    
      public void setComm(int comm) {
        this.comm = comm;
      }
    
      public int getDeptno() {
        return deptno;
      }
    
      public void setDeptno(int deptno) {
        this.deptno = deptno;
      }
    
      @Override
      public String toString() {
        return String.format(
            "Employee [empNo=%d, ename=%s, job=%s, mgr=%d, hiredate=%s, sal=%d, comm=%d, deptno=%d]",
            empNo, ename, job, mgr, hiredate, sal, comm, deptno);
      }
    
    }
  ```
* 이제 HikariCPDemo 클래스의 `main()` 메서드를 실행(run)하면 아래의 결과를 볼 수 있다. 삽입한 데이터가 모두 잘 출력되었다.
  <img width="1186" alt="image" src="https://user-images.githubusercontent.com/49539592/146960997-8fcdcf35-1c67-42d8-9c9e-523b76507f50.png">
* 참고로, 로그를 위로 올리면 우리가 설정한 데이터소스 구성 정보와 그외의 기타 정보를 얻을 수 있다.
  <img width="1480" alt="image" src="https://user-images.githubusercontent.com/49539592/146961199-a6f276ea-5945-4e9f-81c7-303d4ff9bbcb.png">

### 5.5. 테스트
* 마지막으로 간단히 HikariCP 메서드를 테스트하자.
* **HikariCPIntegrationTest.java** (파일 위치는 위의 프로젝트 구조를 참고하자.)
  ```java
    public class HikariCPIntegrationTest {

      @Test
      public void givenConnection_thenFetchDbData() {
        List<Employee> employees = HikariCPDemo.fetchData();
        assertEquals(4, employees.size());
      }
    
    }
  ```
* 위의 코드를 실행하면 아래와 같이 테스트 성공 결과를 볼 수 있다.
  <img width="615" alt="image" src="https://user-images.githubusercontent.com/49539592/146961823-63d34d6d-dd2f-419c-a231-28b8b401ac87.png">


## 참고 자료
* [Introduction to HikariCP](https://www.baeldung.com/hikaricp)
