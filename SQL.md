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

- **AND, OR , NOT  연산자**

  - `WHERE` 절은  `AND`,  `OR`,  `NOT`  연산자와 결합하여 사용할 수 있다.

  - `AND`,  `OR`   연산자는 둘 이상의 조건을 기반으로 레코드를 필터링하는 데 사용된다.

    - `AND`  연산자는  `AND`  양 옆의 조건이 **모두**  `TRUE` 인 레코드를 표시한다.

      > SELECT  column1,  column2,  ...
      >
      > FROM  table_name
      >
      > WHERE  condition1  AND  condition2  AND  condition3  ... ;

    - `OR`  연산자는   `OR`  양 옆의 조건 중 **하나라도**  `TRUE`    인 레코드를 표시한다.

      > SELECT  column1,  column2,  ...
      >
      > FROM  table_name
      >
      > WHERE  condition1  OR   condition2   OR   condition3   ... ;

    - `NOT`  연산자는  `NOT`  다음의 **조건이 일치하지 않는** 레코드를 표시한다. 

      > SELECT  column1,  column2,  ...
      >
      > FROM  table_name
      >
      > WHERE  NOT  condition;

  - `AND`,  `OR`,  `NOT`  연산자를 결합해서 사용할 수도 있다.

    - 다음 SQL 문은 고객 테이블에서 국가가  `독일` 이고,  도시가  `베를린`  또는  `뮌헨` 인  모든 레코드를 얻는다.

      > SELECT  *
      >
      > FROM  Customers
      >
      > WHERE  Country='Germany'   AND  (City='Berlin'  OR  City='München');  

- **ORDER BY  연산자**

  - `ORDER BY`  키워드는 오름차순 또는 내림차순으로 결과물을 정렬하기 위해 사용한다.

  - 오름차순 정렬을 하려면  `ASC`,  내림차순 정렬을 하려면 `DESC`  키워드를 사용한다.  ( **기본 ( `DEFAULT` ) 정렬은 오름차순** 이다. ) 

    > SELECT  column1,  column2,  ...
    >
    > FROM  table_name
    >
    > ORDER  BY  column1,  column2,   ...   ASC|DESC;

  - 여러 개의 컬럼 정렬 사용 예

    > SELECT  *
    >
    > FROM  Customers
    >
    > ORDER BY  Country  AS,  CustomerName; 

- **INSERT INTO 문**

  - `INSERT INTO` 문은 테이블에 새 레코드를 삽입하는 데 사용된다. 아래와 같이 두 가지 방법으로 쿼리를 작성할 수 있다.

    1. 삽입할 컬럼의 이름과 값을 모두 지정한다.

       > INSERT  INTO  table_name  (column1,  column2,  column3,  ... )
       >
       > VALUES  (value1,  value2,  value3,  ... );

    2. 모든 컬럼에 값을 넣는다면 굳이 컬럼의 이름을 나열할 필요 없이 값만 넣어주면 된다. 단, 이때 값의 순서는 컬럼의 순서와 일치해야 한다.

       > INSERT  INTO  table_name
       >
       > VALUES  (value1,  value2,  value3,  ...);

- **NULL 값**

  - `NULL` 값이 있는 필드는 말그대로 **값이 없는 필드** 를 나타낸다.

    > `NULL` 값은  ( 0  또는 공백 ) 과는 다르다.
    >
    > `NULL` 값이 있는 필드는 **생성 중에 비어 있는** 필드를 의미한다.

  - 테이블의 필드가  `Optional` 인 경우 이 필드에 값을 추가하지 않고 새 레코드를 삽입하거나 레코드를 업데이트할 수 있다. 이 경우 필드의 값이 `NULL` 로 저장된다.

  - `NULL` 값 인지의 여부는  오직  `IS NULL`  및  `IS NOT NULL`  연산자를 통해 알 수 있다.

    - ( Tip )  항상  `IS NULL` 을 사용하여  `NULL` 값을 찾도록 하자.

    > SELECT  column_names
    >
    > FROM  table_name
    >
    > WHERE  column_name  IS NULL|IS NOT NULL;
    >
    > (실사용 예시)
    >
    > SELECT  CustomerName,  ContactName,  Address
    >
    > FROM  Customers
    >
    > WHERE  Address  IS NULL;

    > 이때  `=`,  `<`   또는  `<>` 와 같은 비교 연산자를 통해 `NULL` 값인지 알아낼 수 없다.

    

  
# References

- [출처-1 : SQL Tutorial  ( w3schools.com )](https://www.w3schools.com/sql/default.asp) 
