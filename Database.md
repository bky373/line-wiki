# Database

- **트랜잭션** : 데이터베이스 관리 시스템 **상호작용의 단위**이다. 쉽게 데이터베이스의 **상태 변화**를 일으키는 단위라고 볼 수 있다. 
- **트랜잭션의 4대 특징**

  - 원자성 (`Atomicity`)
  - 일관성 (`Consistency`)
  - 독립성 (`Isolation`)
  - 지속성 (`Durability`) 
- **RDBMS**  ( 출처-1 ) 
  - `RDBMS` 는 관계형 데이터베이스 관리 시스템 ( `Relational Database Management System` ) 의 약자이다.
  - `RDBMS` 는 `SQL` 과 `MS SQL Server`,  `IBM DB2`,  `Oracle`,  `MySQL`  및  `Microsoft Access` 와 같은 모든 최신 데이터베이스 시스템의 기반이다.
  - `RDBMS` 의 데이터는 `테이블` 이라는 데이터베이스 개체 ( `object` ) 에 저장된다. `테이블`은 관련 데이터 항목 ( `entries` ) 의 모음이며 열과 행으로 구성된다.
- **필드** ( `Field` ) : 모든 테이블은 필드라고 하는 더 작은 엔티티로 나뉜다. 이 작은 엔티티는 테이블의 **모든 레코드에 대한 특정 정보** 를 유지하도록 설계된 **테이블의 열** ( `column` ) 이기도 하다. 예를 들어 고객 ( `Customer` )  테이블이 있을 때, 이에 대한 필드는 고객 ID ( `CustomerID` ),  고객 이름 ( `CustomerName` ),  주소 ( `Address` )  등으로 구성할 수 있다.
- **레코드** ( `record` ) : 레코드는 테이블의 **수평 엔티티** 이다. **행** ( `row` ) 이라고도 불리며,  테이블에 있는 **각각의 개별 항목** ( `individual entry` ) 을 나타낸다. 예를 들어, 하나의 특정 테이블에 수평 엔티티가 101개 있다고 한다면 다른 말로 101개의 레코드가 있다고 말할 수 있다.

# SQL

- **SQL**  ( 출처-1 )

  -  SQL은 **데이터베이스 접근 및 조작을 위한 표준 언어**이다.  

- **SQL 이란?**

  - `SQL` 은 **구조적 쿼리 언어** ( `Structured Query Language` ) 를 의미한다.
  - `SQL` 은 1986년 `ANSI` ( `American National Standards Institute` ) 의 표준이 되었고,  1987년  `ISO` ( `International Organization for Standardization` ) 의 표준이 되었다.

- **SQL 로 무엇을 할 수 있을까?**

  - `SQL` 은 데이터베이스에 대해 쿼리를 실행할 수 있다.
  - `SQL` 은 새 데이터베이스와 새 테이블을 생성할 수 있다
  - `SQL` 은 생성된 데이터베이스와 테이블에서 데이터 **검색**, 레코드 **삽입** 및 **업데이트**, **삭제** 등을 수행할 수 있다.
  - `SQL` 은 데이터베이스에 대해 저장 프로시저 ( `stored procedures` ) 와 뷰 ( `views` ) 를 생성할 수 있다.
  - `SQL` 은 테이블, 저장 프로시저 및 뷰에 대한 권한을 설정할 수 있다.

- **가장 중요한 SQL 명령들**

  - `SELECT` : 데이터베이스에서 데이터 추출
  - `UPDATE` : 데이터베이스의 데이터 업데이트
  - `DELETE` : 데이터베이스에서 데이터 삭제
  - `INSERT INTO` : 데이터베이스에 새 데이터 삽입
  - `CREATE DATABASE` : 새 데이터베이스 생성
  - `ALTER DATABASE` : 데이터베이스 수정
  - `CREATE TABLE` : 새 테이블 생성
  - `ALTER TABLE` : 테이블 수정
  - `DROP TABLE` : 테이블 삭제
  - `CREATE INDEX` :인덱스 생성 ( 검색 키 )
  - `DROP INDEX` : 인덱스 삭제

- **SELECT DISTINCT 문**

  - 중복값을 제거하고 컬럼의 **고유한(다른)  값** 을 얻기 위해 사용된다.

    > SELECT  DISTINCT   column1,  column2, ... 
    >
    > FROM   table_name;
    >
    > 
    >
    >
    > --  다음의 SQL 문은 고객 테이블에서 서로 다른(고유한) 국가의 수를 반환한다.
    >
    > SELECT  COUNT(DISTINCT  Country)  
    >
    > FROM  Customers; 
    >
    > --  결과
    >
    > 컬럼 이름 : COUNT(DISTINCT Country)  
    >
    > 값 : 21

    

    > --  위와 동일한 값을 얻으면서 동시에 컬럼의 이름을 변경한 쿼리는 다음과 같다.
    >
    > SELECT  Count(*)  AS  DistinctCountries
    >
    > FROM  (SELECT  DISTINCT  Country  FROM  Customers);
    >
    > --  결과
    >
    > 컬럼 이름 : DistinctCountries   
    >
    > 값 : 21

- **WHERE 문**

  - `WHERE` 절은 레코드를 필터링하는 데 사용된다.

  - `WHERE` 다음에 지정된 조건을 만족하는 레코드만 추출한다. 

    > SELECT  column1,  column2,  ...
    >
    > FROM  table_name
    >
    > WHERE  condition;
    >
    > - `WHERE` 절은  `SELECT`  문 이외에도  `UPDATE`,  `DELETE` 등에서 사용된다.

  - SQL에서는 텍스트 값을 작은 따옴표 ( `' '` ) 로 묶어야 한다. ( 대부분의 데이터베이스 시스템에서는 큰 따옴표 ( `"  "` ) 도 허용한다. )  

  - 하지만 숫자 필드는 따옴표로 묶지 않고 사용한다.

    > SELECT  *  FROM  Customers
    >
    > WHERE  CustomerID=1;

- **WHERE 절의 연산자**

  | **연산**                | **설명**                                                     |
  | ----------------------- | ------------------------------------------------------------ |
  | **A  =  B**             | **A가  B와 동일한 것을 반환함**                              |
  | **A  >  B**             | **A가  B보다 큰 것을 반환함**                                |
  | **A  <  B**             | **A가  B보다 작은 것을 반환함**                              |
  | **A  >=  B**            | A가  B와 같거나 B보다 큰 것을 반환함                         |
  | **A  <=  B**            | A가  B와 같거나 B보다 작은 것을 반환함                       |
  | **A  <>  B**            | A가  B와 다른 것을 반홤함 <br />`NOT A = B`  을 사용할 수 있음<br />일부 SQL 버전에서는   `!=`   연산자를 사용함 |
  | **BETWEEN   A  AND  B** | A와 B 사이에 있는 것을 반환함  <br />(  A와 B 모두 포함  )   |
  | **LIKE  A**             | A라는 패턴에 해당하는 것을 반환함<br />`%` 연산을 사용하여 패턴을 만들 수 있음  <br />( `예)  첫 글자가 s로 시작하는 값 검색 -> [컬럼 이름]  LIKE  's%'`  ) |
  | **IN  ( A,  B,  ... )** | 컬럼에 대해 여러 값을 찾음<br />(  A,  B,  ...  ) 등 각각을 비교값으로 보고 일치하는 값을 찾음<br />여러 개의 값을 하나의 괄호 안에 묶어주어야 함 |

  


# References

- [출처-1 : SQL Tutorial  ( w3schools.com )](https://www.w3schools.com/sql/default.asp) 
