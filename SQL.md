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

    > **SELECT  DISTINCT**   column1,  column2, ... 
    >
    > **FROM**   table_name;
    >
    > 
    >
    >
    > --  다음의 SQL 문은 고객 테이블에서 서로 다른(고유한) 국가의 수를 반환한다.
    >
    > **SELECT**  **COUNT**(**DISTINCT**  Country)  
    >
    > **FROM**  Customers; 
    >
    > --  결과
    >
    > 컬럼 이름 : COUNT(DISTINCT Country)  
    >
    > 값 : 21

    

    > --  위와 동일한 값을 얻으면서 동시에 컬럼의 이름을 변경한 쿼리는 다음과 같다.
    >
    > **SELECT**  **COUNT**(*)  **AS**  DistinctCountries
    >
    > **FROM**  (**SELECT  DISTINCT**  Country  **FROM**  Customers);
    >
    > --  결과
    >
    > 컬럼 이름 : DistinctCountries   
    >
    > 값 : 21

- **WHERE 절**

  - `WHERE` 절은 레코드를 필터링하는 데 사용된다.

  - `WHERE` 다음에 지정된 조건을 만족하는 레코드만 추출한다. 

    > **SELECT**  column1,  column2,  ...
    >
    > **FROM**  table_name
    >
    > **WHERE**  condition;
    >
    > - `WHERE` 절은  `SELECT`  문 이외에도  `UPDATE`,  `DELETE` 등에서 사용된다.

  - SQL에서는 텍스트 값을 작은 따옴표 ( `' '` ) 로 묶어야 한다. ( 대부분의 데이터베이스 시스템에서는 큰 따옴표 ( `"  "` ) 도 허용한다. )  

  - 하지만 숫자 필드는 따옴표로 묶지 않고 사용한다.

    > **SELECT**  *  
    >
    > **FROM**  Customers
    >
    > **WHERE**  CustomerID=1;

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

      > **SELECT**  column1,  column2,  ...
      >
      > **FROM**  table_name
      >
      > **WHERE**  condition1  AND  condition2  AND  condition3  ... ;

    - `OR`  연산자는   `OR`  양 옆의 조건 중 **하나라도**  `TRUE`    인 레코드를 표시한다.

      > **SELECT**  column1,  column2,  ...
      >
      > **FROM**  table_name
      >
      > **WHERE**  condition1  OR   condition2   OR   condition3   ... ;

    - `NOT`  연산자는  `NOT`  다음의 **조건이 일치하지 않는** 레코드를 표시한다. 

      > **SELECT**  column1,  column2,  ...
      >
      > **FROM**  table_name
      >
      > **WHERE  NOT**  condition;

  - `AND`,  `OR`,  `NOT`  연산자를 결합해서 사용할 수도 있다.

    - 다음 SQL 문은 고객 테이블에서 국가가  `독일` 이고,  도시가  `베를린`  또는  `뮌헨` 인  모든 레코드를 얻는다.

      > **SELECT**  *
      >
      > **FROM**  Customers
      >
      > **WHERE**  Country='Germany'   AND  (City='Berlin'  OR  City='München');  

- **ORDER BY  연산자**

  - `ORDER BY`  키워드는 오름차순 또는 내림차순으로 결과물을 정렬하기 위해 사용한다.

  - 오름차순 정렬을 하려면  `ASC`,  내림차순 정렬을 하려면 `DESC`  키워드를 사용한다.  ( **기본 ( `DEFAULT` ) 정렬은 오름차순** 이다. ) 

    > **SELECT**  column1,  column2,  ...
    >
    > **FROM**  table_name
    >
    > **ORDER  BY**  column1,  column2,   ...   ASC|DESC;

  - 여러 개의 컬럼 정렬 사용 예

    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **ORDER BY**  Country  ASC,  CustomerName; 

- **INSERT INTO 문**

  - `INSERT INTO` 문은 테이블에 새 레코드를 삽입하는 데 사용된다. 아래와 같이 두 가지 방법으로 쿼리를 작성할 수 있다.

    1. 삽입할 컬럼의 이름과 값을 모두 지정한다.

       > **INSERT  INTO**  table_name  (column1,  column2,  column3,  ... )
       >
       > **VALUES**  (value1,  value2,  value3,  ... );

    2. 모든 컬럼에 값을 넣는다면 굳이 컬럼의 이름을 나열할 필요 없이 값만 넣어주면 된다. 단, 이때 값의 순서는 컬럼의 순서와 일치해야 한다.

       > **INSERT  INTO**  table_name
       >
       > **VALUES**  (value1,  value2,  value3,  ...);

- **NULL 값**

  - `NULL` 값이 있는 필드는 말그대로 **값이 없는 필드** 를 나타낸다.

    > `NULL` 값은  ( 0  또는 공백 ) 과는 다르다.
    >
    > `NULL` 값이 있는 필드는 **생성 중에 비어 있는** 필드를 의미한다.

  - 테이블의 필드가  `Optional` 인 경우 이 필드에 값을 추가하지 않고 새 레코드를 삽입하거나 레코드를 업데이트할 수 있다. 이 경우 필드의 값이 `NULL` 로 저장된다.

  - `NULL` 값 인지의 여부는  오직  `IS NULL`  및  `IS NOT NULL`  연산자를 통해 알 수 있다.

    - ( Tip )  항상  `IS NULL` 을 사용하여  `NULL` 값을 찾도록 하자.

    > **SELECT**  column_names
    >
    > **FROM**  table_name
    >
    > **WHERE**  column_name  IS NULL|IS NOT NULL;
    >
    > (실제 사용 예시)
    >
    > **SELECT**  CustomerName,  ContactName,  Address
    >
    > **FROM**  Customers
    >
    > **WHERE**  Address  IS NULL;

    > 이때  `=`,  `<`   또는  `<>` 와 같은 비교 연산자를 통해 `NULL` 값인지 알아낼 수 없다.

- **UPDATE 문**

  - `UPDATE` 문은 테이블의 기존 레코드를 수정하기 위해 사용한다.

    - ( 주의! )  `UPDATE` 문을 사용할 때 업데이트해야 하는 레코드를 **`WHERE` 절을 통해 지정** 해야 한다.  만약  `WHERE` 절을 생략하면 테이블의 모든 레코드가 업데이트된다!

    > **UPDATE**  table_name
    >
    > **SET**  column1=value1,  column2=value2,  ...
    >
    > **WHERE**  condition;
    >
    > (실제 사용 예시)
    >
    > **UPDATE**  Customers
    >
    > **SET**  ContactName='Alfred Schmidt',  City='Frankfurt'
    >
    > **WHERE**  CustomerID = 1;

  - `WHERE` 절은 업데이트할 레코드 수를 결정한다. 아래 SQL 문은 국가가 'Mexico' 인 모든 레코드의 `ContactName` 을  'Juan' 으로 업데이트한다.

    > **UPDATE** Customers
    > **SET** ContactName='Juan'
    > **WHERE** Country='Mexico';

- **DELETE 문**

  - `DELETE` 문은 테이블의 기존 레코드를 삭제하기 위해 사용한다.

  - ( 주의! )  **`WHERE` 절** 을 생략하면 테이블의 모든 레코드가 삭제된다.

    > **DELETE FROM**  table_name
    >
    > **WHERE**  condition;
    >
    > (실사용 예시)
    >
    > **DELETE FROM** Customers 
    >
    > **WHERE** CustomerName='Peter Parker';

- **SELECT TOP 또는 LIMIT 절**

  - `SELECT TOP` 절은 반환할 레코드 수를 지정하는 데 사용된다.

  - `SELECT TOP` 절은 수천 개의 레코드가 있는 큰 테이블에서 유용하다.  목적 없이 많은 수의 레코드를 반환하면 성능에 영향을 주기 때문이다.

  - 다만 모든 데이터베이스 시스템이 `SELECT TOP`  절을 지원하지는 않는다. `MySQL` 은 제한된 수의 레코드를 선택하기 위해 `LIMIT` 을 사용하지만,  `Oracle` 은 `FETCH FIRST n ROWS ONLY`  또는  `ROWNUM` 을 사용한다.

  - **MySQL 문법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > ( **WHERE** condition )
    >
    > **LIMIT** number;
    >
    > 
    >
    >
    > (실사용 예시)
    >
    > **SELECT** *
    >
    > **FROM** Customers
    >
    > **LIMIT** 3;

  - **SQL Server / MS Access 문법**

    > **SELETC TOP** number|**PERCENT** column_name(s)
    >
    > **FROM**  table_name
    >
    > **WHERE**  condition;

  - **Oracle 12 문법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **ORDER BY** column_name(s)
    >
    > **FETCH** **FIRST**  number|**PERCENT**  **ROWS ONLY**;

  - **이전 Oracle 문법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** ROWNUM <= number;

  - 숫자 뒤에 `PERCENT` 키워드를 작성하는 경우, 해당 숫자의 퍼센트 만큼 레코드를 반환한다.

    > -- 총 레코드 수(91개)의 절반인 46개의 레코드를 반환 
    >
    > **SELECT TOP** 50 **PERCENT** * 
    >
    > **FROM** Customers;

- **SQL MIN  /  MAX 함수**

  - `MIN()` 함수는 선택한 열의 가장 작은 값을 반환한다.

    > **SELECT MIN**(column_name)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

  - `MAX()` 함수는 선택한 열의 가장 큰 값을 반환한다.

    > **SELECT MAX**(column_name)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

- **SQL COUNT  /  AVG  /  SUM 함수**

  - `COUNT()` 함수는 지정된 기준과 일치하는 행의 수를 반환한다.

    > **SELECT COUNT**(column_name)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

  - `AVG()` 함수는 숫자로 구성된 열의 평균 값을 반환한다.

    > **SELECT AVG**(column_name)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

  - `SUM()` 함수는 숫자로 구성된 열의 총합을 반환한다.

    > **SELECT SUM**(column_name)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

- **LIKE 연산자** : `LIKE` 연산자는 `WHERE` 절에서 열에서 지정한 패턴을 검색하는 데 사용된다.

  > **SELECT** column1, column2, ...
  >
  > **FROM**  table_name
  >
  > **WHERE** columnN  **LIKE** pattern;

  - 아래 두 개의 **와일드카드**는  `LIKE` 연산자와  자주 사용된다.

    > **퍼센트 기호 ( `%` )** : 0, 1 또는 여러 개의 문자를 나타낸다.
    >
    > **밑줄 기호 ( `_` )** : 하나의 단일 문자를 나타낸다.
    >
    > - 참고로  `MS Access`는 퍼센트 기호 ( `%` )  대신 별표 ( `*` )를 사용하고,  밑줄 ( `_` ) 대신 물음표 ( `?` )를 사용한다.

  - `LIKE` 연산자와  `%`  및  `_`  와일드카드의 사용 예

    | LIKE 연산자                               | 설명                                              |
    | ----------------------------------------- | ------------------------------------------------- |
    | **WHERE** CustomerName **LIKE** 'a%'      | `a`로 시작하는 모든 값을 찾는다.                  |
    | **WHERE** CustomerName **LIKE** '%a'      | `a`로 끝나는 모든 값을 찾는다.                    |
    | **WHERE** CustomerName **LIKE** '%or%'    | 위치에 상관없이 `or` 를 포함하는 값을 찾는다.     |
    | **WHERE** CustomerName **LIKE** '_r%'     | 두 번째 위치에  `r` 이 있는 값을 찾는다.          |
    | **WHERE** CustomerName **LIKE** 'a_%'     | `a`로 시작하고 길이가 2자 이상인 값을 찾는다.     |
    | **WHERE** CustomerName **LIKE** 'a__%'    | `a`로 시작하고 길이가 3자 이상인 값을 찾는다.     |
    | **WHERE** CustomerName **LIKE** 'a%z'     | `a`로 시작하고 `z`로 끝나는 값을 찾는다.          |
    | **WHERE** CustomerName **LIKE** '[ace]%'  | `a`, `c` 또는 `e` 로 시작하는 값을 찾는다.        |
    | **WHERE** CustomerName **LIKE** '[a-f]%'  | `a` 부터  `f` 사이의 문자로 시작하는 값을 찾는다. |
    | **WHERE** CustomerName **LIKE** '[!ace]%' | `a`, `c` 또는 `e` 로 시작하지 않는 값을 찾는다.   |

- **SQL 와일드카드 문자**

  - 와일드카드 문자는 문자열에서 하나 이상의 문자를 대체하기 위해 사용한다.

  - 와일드카드 문자는 `LIKE` 연산자와 함께 사용된다. `LIKE` 연산자는 `WHERE` 절에서 열에서 지정된 패턴을 검색하는 데 사용된다.

  - 와일드카드 문자는 서로 조합하여 함께 사용할 수 있다.

  - **`MS Access`의 와일드카드 문자**

    | 기호     | 설명                                                     | 예시                                                         |
    | -------- | -------------------------------------------------------- | ------------------------------------------------------------ |
    | **`*`**  | 0개 이상의 문자를 나타낸다.                              | `bl*` 은  `bl`, `black`, `blue`, `blob` 등을 찾는다.         |
    | **`?`**  | 단일 문자를 나타낸다.                                    | `h?t` 는  `hot`, `hat`, `hit` 등을 찾는다                    |
    | **`[]`** | 대괄호( `[]` ) 안에 있는 단일 문자를 나타낸다.           | `h[oa]t` 는  `hot`,  `hat` 을 찾지만,  `hit` 은 찾지 못한다. |
    | **`!`**  | 대괄호( `[]` ) 안에 있는 문자 외의 단일 문자를 나타낸다. | `h[!oa]t` 는  `hit`은 찾지만,  `hot` 과  `hat` 은 찾지 못한다. |
    | **`-`**  | 문자 범위를 나타낸다.                                    | `c[a-c]t` 는  `cat` 와 `cbt`,  `cct` 를 찾는다.              |
    | **`#`**  | 단일 숫자 문자를 나타낸다.                               | `2#5`는  `205`, `215`, `225`, `235`, `245`, `255`, `265`, `275`, `285`, `295`를 찾는다. |

  - **`SQL Server`의 와일드카드 문자**

    | 기호 | 설명                                                     | 예시                                                         |
    | ---- | -------------------------------------------------------- | ------------------------------------------------------------ |
    | `%`  | 0개 이상의 문자를 나타낸다.                              | `bl%` 은  `bl`, `black`, `blue`, `blob` 등을 찾는다.         |
    | `-`  | 단일 문자를 나타낸다.                                    | `h_t` 는  `hot`, `hat`, `hit` 등을 찾는다                    |
    | `[]` | 대괄호( `[]` ) 안에 있는 단일 문자를 나타낸다.           | `h[oa]t` 는  `hot`,  `hat` 을 찾지만,  `hit` 은 찾지 못한다. |
    | `^`  | 대괄호( `[]` ) 안에 있는 문자 외의 단일 문자를 나타낸다. | `h[^oa]t` 는  `hit`은 찾지만,  `hot` 과  `hat` 은 찾지 못한다. |
    | `-`  | 문자 범위를 나타낸다.                                    | `c[a-c]t` 는  `cat` 와 `cbt`,  `cct` 를 찾는다.              |

- **IN 연산자**

  - `IN` 연산자를 사용하면  `WHERE` 절에 **여러 개의 값** 을 지정할 수 있다.

  - `IN` 연산자는 여러 개의  `OR` 조건의 약어이다.

    > **SELECT**  column_name(s)
    >
    > **FROM**  table_name
    >
    > **WHERE**  column_name **IN** (value1, value2, ...);
    >
    > 
    >
    > (실사용 예시)
    >
    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **WHERE**  Country  **IN** ('Germany', 'France', 'UK');
    >
    > 
    >
    > ( **NOT 연산자** 와 함께 )
    >
    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **WHERE**  Country  **NOT IN** ('Germany', 'France', 'UK');

    또는

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** column_name **IN** (**SELECT** STATEMENT);
    >
    > 
    >
    > (실사용 예시)
    >
    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **WHERE**  Country  **IN** (**SELECT** Country **FROM** Suppliers);

- **BETWEEN 연산자**

  - `BETWEEN` 연산자는 **주어진 범위** 내에서 값을 선택한다. 값은 **숫자, 텍스트 또는 날짜** 일 수 있다.

  - `BETWEEN` 연산자는 **`inclusive`** 하다. 따라서 **시작 값과 마지막 값이 포함** 된다.

    > **SELECT**  column_name(s)
    >
    > **FROM**  table_name
    >
    > **WHERE**  column_name  **BETWEEN**  value1  **AND** value2;
    >
    > (실사용 예시)
    >
    > **SELECT**  *
    >
    > **FROM**  Products
    >
    > **WHERE**  Price **BETWEEN** 10 **AND** 20;

  - **NOT BETWEEN 예시**

    > **SELECT**  *
    >
    > **FROM**  Products
    >
    > **WHERE**  Price **NOT BETWEEN** 10 **AND** 20;

  - **BETWEEN** 과  **IN** 예시

    > **SELECT**  *
    >
    > **FROM**  Products
    >
    > **WHERE**  Price **BETWEEN** 10 **AND** 20
    >
    > **AND** CategoryID **NOT IN** (1, 2, 3);

  - **BETWEEN 텍스트 값 예시**

    - 다음 SQL 문은  `Carnarvon Tigers`와  `Chef Anton's Cajun Seasoning` 사이의 `ProductName` 값을 갖는 모든 레코드를 반환한다. 

    > **SELECT** * 
    >
    > **FROM** Products
    >
    > **WHERE** ProductName **BETWEEN** "Carnarvon Tigers" **AND** "Chef Anton's Cajun Seasoning"
    > **ORDER BY** ProductName;

    ![image](https://user-images.githubusercontent.com/49539592/127869665-09f0f3a4-fdc8-4f86-b0e7-8e6cd96d1f9a.png)

  - **BETWEEN 날짜 예시**

    - 다음 SQL 문은  `OrderDate`가  `'01-July-1996'` 과  `'31-July-1996'`  사이에 있는 모든 주문을 선택한다.

      > **SELECT** *
      >
      > **FROM** Orders
      >
      > **WHERE** OrderDate **BETWEEN** \#07/01/1996# **AND** #07/31/1996#;

      또는

      > **SELECT** *
      >
      > **FROM** Orders
      >
      > **WHERE** OrderDate
      >
      > **BETWEEN** `1996-07-01` **AND**  `1996-07-31`;

- **SQL Aliases ( 별칭 )**

  - SQL 별칭은 **테이블** 이나 **테이블의 열** 에  **임시 이름** 을 지정하는 데 사용된다.

  - 별칭은 `AS` 키워드를 통해 생성되며, **해당 쿼리 기간 동안에만** 존재한다.

  - **컬럼의 별칭 사용**

    > **SELECT** column_name **AS** alias_name
    >
    > **FROM** table_name;
    >
    > 
    >
    > (실사용 예시)
    >
    > **SELECT** CustomerID **AS** ID, CustomerName **AS** Customer
    >
    > **FROM** Customers;

    ![image](https://user-images.githubusercontent.com/49539592/127871370-711e0485-0dbc-4f5d-8ced-15e344a1f308.png)

    

  - 별칭 이름에 **공백** 이 포함된 경우,  **큰 따옴표** 나 **대괄호** 를 사용해야 한다.

    > **SELECT** CustomerID **AS** ID, ContactName **AS** [Contact Person]
    >
    > **FROM** Customers;

    ![image](https://user-images.githubusercontent.com/49539592/127871868-f31caefa-f2e0-44b8-b6da-3baf99d7ee94.png)

  - 다음 SQL 문은 4개의 열(`Address`, `PostalCode`, `City` 및 `Country`)을 결합하여 "`Address`"라는 별칭을 만든다. 

    > **SELECT** CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country **AS** Address
    > **FROM** Customers;
    >
    > 
    >
    > -- 아래 SQL문은 `MySQL` 기준
    >
    > **SELECT** CustomerName, **CONCAT**(Address, ', ', PostalCode, ', ', City, ', ', Country) **AS** Address
    > **FROM** Customers;

  - **테이블의 별칭 사용**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name **AS** alias_name;
    >
    > (실사용 예시)
    >
    > **SELECT** o.OrderID, o.OrderDate, c.CustomerName
    > **FROM** Customers **AS** c, Orders **AS** o
    > **WHERE** c.CustomerName='Blondel père et fils' **AND** c.CustomerID=o.CustomerID;

    - 위의 SQL 문은 `CustomerName`이  `Blondel père et fils`이고,  `CustomerID`가  7번인 고객의 모든 주문을 선택한다. 이때 `Customers` 와  `Orders` 테이블이 사용되는데,  각각  `c` 와  `o`라는 별칭을 갖는다.
    - 여기서 `c` 와  `o`라는 별칭을 사용함으로써  SQL 문을 더 짧게 만들어 사용할 수 있다.

- **SQL JOIN 절**

  - `JOIN` 절은 두 개 이상의 테이블에서, 서로 연결된 열에 근거하여 관련된 행들을 결합하기 위해 사용한다.

  - 예를 들어, 아래 두 가지 테이블 및 컬럼 정보가 있다.

    - `Orders` 테이블의 컬럼 : `OrderID`,  `CustomerID`,  `OrderDate`

      ![image](https://user-images.githubusercontent.com/49539592/128020423-72fab655-825c-4824-9dca-f95075ec8bc9.png)

    - `Customers` 테이블의 컬럼 : `CustomerID`,  `CustomerName`,  `ContactName`,  `Country`

      ![image](https://user-images.githubusercontent.com/49539592/128021072-6a88ba40-9982-4f6b-b238-0b140cd0f7d4.png)

  - 여기에서 `Orders` 테이블의 `CustomerID` 열은,  `Customers` 테이블의 `CustomerID`를 참조한다. 따라서 위의 두 테이블에서 `CustomerID` 열은 서로 관계가 있다고 할 수 있다.

  - 그렇다면, 다음 SQL 문 ( `INNER JOIN` )에서는, `CustomerID` 열을 기준으로 두 테이블에서 일치하는 레코드 값을 얻을 수 있다.

    > **SELECT** O.OrderID, C.CustomerName, O.OrderDate
    >
    > **FROM** Orders **AS** O
    >
    > **INNER JOIN** Customers **AS** C 
    >
    > **ON** O.CustomerID=C.CustomerID;

    ![image](https://user-images.githubusercontent.com/49539592/128021005-99b67201-f350-460d-a69b-a55781d9db3b.png)

- **SQL JOIN의 다양한 타입**

  - `(INNER) JOIN` : 두 테이블에서 **일치하는 값** 을 가진 레코드를 반환한다.

    > 결과적으로 **왼쪽 테이블과 오른쪽 테이블의 접점** 을 가진 레코드들을 결합하여 반환한다.

  - `LEFT (OUTER) JOIN` : **왼쪽 테이블의 모든 레코드** 를 반환하고, 왼쪽 테이블 컬럼의 값과 일치하는 **오른쪽 테이블의 레코드를 결합** 하여 반환한다.

    > - 왼쪽 테이블 컬럼의 값 하나가 있고, 이 값과 일치하는 오른쪽 테이블 컬럼의 레코드가 여러 개 있을 경우, 오른쪽 테이블 레코드의 수만큼 행이 만들어진다.
    >
    > - 만약 오른쪽 테이블에 일치하는 항목이 없다면 왼쪽 테이블의 레코드의 수만큼 행을 반환한다.

  - `RIGHT (OUTER) JOIN` : **오른쪽 테이블의 모든 레코드** 를 반환하고, 왼쪽 테이블에서는 오른쪽 테이블 컬럼의 값과 일치하는 레코드를 결합하여 반환한다.

    > - 오른쪽 테이블 컬럼의 값 하나가 있고, 이 값과 일치하는 왼쪽 테이블 컬럼의 레코드가 여러 개 있을 경우, 왼쪽 테이블 레코드의 수만큼 행이 만들어진다.
    >
    > - 만약 왼쪽 테이블에 일치하는 항목이 없다면 오른쪽 테이블의 레코드의 수만큼 행을 반환한다.

  - `FULL (OUTER) JOIN` : **왼쪽 또는 오른쪽 테이블** 에 일치하는 항목이 있는 경우 서로 결합하여 모든 레코드를 반환한다.

    > 결과적으로 **왼쪽 테이블과 오른쪽 테이블에서 일치하는 값들을 모두 결합시킨 레코드** 를 반환한다.

  ![image](https://user-images.githubusercontent.com/49539592/128021848-cad68bc8-6ceb-405c-ba4d-3458351518c6.png)

- **INNER JOIN 키워드**

  - `INNER JOIN` 키워드는 두 테이블에서 일치하는 값을 가진 레코드를 선택한다.

    > **SELECT** column_name(s)
    > **FROM** table1
    > **INNER JOIN** table2
    > **ON** table1.column_name = table2.column_name;
    >
    > (실사용 예시)
    >
    > **SELECT** Orders.OrderID, Customers.CustomerName
    > **FROM** Orders
    > **INNER JOIN** Customers 
    >
    > **ON** Orders.CustomerID = Customers.CustomerID;

  - 다음 SQL 문은 세 개의 테이블을 조인한다.

    > **SELECT** Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
    > **FROM** ( (Orders
    > **INNER JOIN** Customers 
    >
    > **ON** Orders.CustomerID = Customers.CustomerID)
    > **INNER JOIN** Shippers 
    >
    > **ON** Orders.ShipperID = Shippers.ShipperID );

- **LEFT (OUTER) JOIN 키워드**

  - `LEFT JOIN` 키워드는 왼쪽 테이블(table1)의 모든 레코드와 오른쪽 테이블(table2)의 일치하는 레코드를 반환한다. 

  - 일치하는 항목이 없는 경우 오른쪽으로부터는 아무 레코드도 결합하지 않는다. ( 그래도 굳이 오른쪽 테이블의 컬럼을 표시한다면 해당 컬럼의 레코드는 `null` 값을 갖는다 )

    > **SELECT** column_name(s)
    >
    > **FROM** table1
    >
    > **LEFT JOIN** table2
    >
    > **ON** table1.column_name = table2.column_name;
    >
    > (실사용 예시)
    >
    > **SELECT** Customers.CustomerName, Orders.OrderID
    > **FROM** Customers
    > **LEFT JOIN** Orders
    > **ON** Customers.CustomerID=Orders.CustomerID
    > **ORDER BY** Customers.CustomerName;

    ![image](https://user-images.githubusercontent.com/49539592/128034193-da548c93-8662-4d38-a1f7-de7cfc985291.png)

- **RIGHT (OUTER) JOIN 키워드**

  - `RIGHT JOIN` 키워드는 오른쪽 테이블(table2)의 모든 레코드와 왼쪽 테이블(table1)의 일치하는 레코드를 반환한다. 

  - 일치하는 항목이 없는 경우 왼쪽으로부터는 아무 레코드도 결합하지 않는다. ( 그래도 굳이 왼쪽 테이블의 컬럼을 표시한다면 해당 컬럼의 레코드는 `null` 값을 갖는다 )  

    > **SELECT** column_name(s)
    > **FROM** table1
    > **RIGHT JOIN** table2
    > **ON** table1.column_name = table2.column_name;
    >
    > (실사용 예시)
    >
    > **SELECT** Orders.OrderID, Employees.LastName, Employees.FirstName
    >
    > **FROM** Orders
    >
    > **RIGHT JOIN** Employees 
    >
    > **ON** Orders.EmployeeID = Employees.EmployeeID
    >
    > **ORDER BY** Orders.OrderID;

- **FULL (OUTER) JOIN 키워드**

  - `FULL OUTER JOIN` 키워드는 왼쪽(table1) 또는 오른쪽(table2) 테이블 레코드에 일치 항목이 있는 경우 모든 레코드를 반환한다.

  - ( 주의! ) `FULL OUTER JOIN`은 잠재적으로 매우 거대한 레코드 집합을 결과값으로 반환할 수 있다.

    > **SELECT** column_name(s)
    >
    > **FROM** table1
    >
    > **FULL OUTER JOIN** table2
    >
    > **ON** table1.column_name = table2.column_name
    >
    > **WHERE** condition;
    >
    > (실사용 예시)
    >
    > **SELECT** Customers.CustomerName, Orders.OrderID
    >
    > **FROM** Customers
    >
    > **FULL OUTER JOIN** Orders 
    >
    > **ON** Customers.CustomerID=Orders.CustomerID
    >
    > **ORDER BY** Customers.CustomerName;

- **SQL SELF JOIN**

  - `SELF JOIN` 은 이름에서처럼 일반적인 조인의 기능을 수행하지만, 테이블 자신을 조인한다는 점에서 다른 조인과 다르다.

  - 아래 SQL 문에서 `T1`과  `T2`는 같은 테이블을 각각 다른 별칭으로 지정한 것이다.

    > **SELECT** column_name(s)
    >
    > **FROM** table1 T1,  table2 T2
    >
    > **WHERE** condition;
    >
    > (실사용 예시)
    >
    > **SELECT** A.City, A.CustomerName **AS** CustomerName1, B.CustomerName **AS** CustomerName2
    >
    > **FROM** Customers A, Customers B
    >
    > **WHERE** A.CustomerID <> B.CustomerID
    >
    > **AND** A.City = B.City
    >
    > **ORDER BY** A.City;

  - 위의 SQL 문은 같은 도시에 있는 다른 고객들을 (도시를 기준으로) 결합시켜 이름을 나타낸 것이다.

    ![image](https://user-images.githubusercontent.com/49539592/128044922-3233b8ed-4a77-4db5-9e32-10ba8cc2a3dc.png)

- **SQL UNION 연산자**

  - `UNION` 연산자는 **두 개 이상의 `SELECT` 문의 결과 집합을 결합** 하는 데 사용된다.

    - `UNION` 내의 모든 `SELECT` 문에는 **동일한 수의 컬럼** 이 있어야 한다. 
    - 컬럼끼리는 **유사한 데이터 타입** 을 가지고 있어야 한다. 
    - 모든 `SELECT` 문의 컬럼은 **같은 순서** 로 나열되어야 한다.

    > **SELECT** column_name(s) **FROM** table1
    >
    > **UNION**
    >
    > **SELECT** column_name(S) **FROM** table2;

- **UNION ALL 구문**

  - `UNION` 연산자는 기본적으로 **고유한** ( `distinct` )  값만 선택한다. 

  - **중복 값** 을 허용하려면 `UNION ALL` 을 사용해야 한다.

    > **SELECT** column_name(s) **FROM** table1
    >
    > **UNION ALL**
    >
    > **SELECT** column_name(s) **FROM** table2;

  - ( 주의! ) 결과 집합의 열 이름은 일반적으로 첫 번째 `SELECT` 문의 열 이름과 같다.

  - 다음 SQL 문은 모든 고객과 공급업체를 나열한다.

    > **SELECT** 'Customer' **AS** Type, ContactName, City, Country
    >
    > **FROM** Customers
    >
    > **UNION**
    >
    > **SELECT** 'Supplier', ContactName, City, Country
    >
    > **FROM** Suppliers;

    ![image](https://user-images.githubusercontent.com/87057868/128156725-b7e91501-ff3a-4038-b4fa-4ad900107c62.png)

- **GROUP BY 문**

  - `GROUP BY` 문은  `"각 국가의 고객 수 찾기"` 와 같이 **동일한 값을 가진 행을 요약 행으로 그룹화** 한다. 
  - `GROUP BY` 문은 하나 이상의 열로 결과 집합을 그룹화하기 위해 **집계 함수** ( `COUNT()`, `MAX()`, `MIN()`, `SUM()`, `AVG()` ) 와 함께 자주 사용된다.

  > **SELECT** column_name(s)
  >
  > **FROM** table_name
  >
  > **WHERE** condition
  >
  > **GROUP BY** column_name(s)
  >
  > **ORDER BY** column_name(s);
  >
  > (실사용 예시)
  >
  > **SELECT** S.ShipperName, **COUNT**(O.OrderID) **AS** NumberOfOrders 
  >
  > **FROM** Orders **AS** O
  >
  > **LEFT JOIN** Shippers **AS** S **ON** O.ShipperID=S.ShipperID
  >
  > **GROUP BY** ShipperName;

- **SQL HAVING 절**

  - `WHERE` 키워드는 집계 함수와 함께 사용할 수 없기 때문에 `HAVING` 절이 SQL에 추가되었다.

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** condition
    >
    > **GROUP BY** column_name(s)
    >
    > **HAVING** condition
    >
    > **ORDER BY** column_name(s);

  - 다음 SQL 문은 각 국가의 고객의 수를 나열한다. 이때 고객이 5명 이상인 나라만 포함한다.

    > **SELECT COUNT**(CustomerID), Country
    >
    > **FROM** Customers
    >
    > **GROUP BY** Country
    >
    > **HAVING COUNT**(CustomerID) > 5;

  - 다음 SQL 문은 10개 이상의 주문을 등록한 직원을 주문 순서가 많은 순으로 나열한다.

    > **SELECT** E.LastName, **COUNT**(O.OrderID) AS NumberOfOrders
    >
    > **FROM** Orders **AS** O
    >
    > **INNER JOIN** Employees **AS** E 
    >
    > **ON** O.EmployeeID = E.EmployeeID
    >
    > **GROUP BY** LastName
    >
    > **HAVING COUNT**(O.OrderID) > 10
    >
    > **ORDER BY COUNT**(O.OrderID) **DESC;**

  - 다음 SQL 문은 직원 `"Davolio"` 또는 `"Fuller"` 가 25개 이상의 주문을 등록했는지 여부를 나열한다.

    > **SELECT** E.LastName, **COUNT(**O.OrderID) **AS** NumberOfOrders
    >
    > **FROM** Orders
    >
    > **INNER JOIN** Employees
    >
    > **ON** O.EmployeeID = E.EmployeeID
    >
    > **WHERE** LastName='Davolio' **OR** LastName='Fuller'
    >
    > **GROUP BY** LastName
    >
    > **HAVING COUNT**(O.OrderID) > 25;

- **SQL EXISTS 연산자**

  - `EXISTS` 연산자는 **하위 쿼리에 레코드가 있는지** 테스트하는 데 사용된다.

  - `EXISTS` 연산자는 하위 쿼리가 **하나 이상의 레코드** 를 반환하는 경우 **`TRUE`를 반환** 한다.

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE EXISTS**
    >
    > (**SELECT** column_name **FROM** table_name **WHERE** condition);

  - 다음 SQL 문은 `TRUE` 를 반환하고 제품 가격이 20 미만인 공급자를 나열한다.

    > **SELECT** SupplierName
    >
    > **FROM** Suppliers
    >
    > **WHERE EXISTS** (**SELECT** ProductName **FROM** Products **WHERE** Products.SupplierID = Suppliers.supplier **AND** Price < 20)

  - 다음 SQL 문은 `TRUE`를 반환하고 제품 가격이 22인 공급자를 나열한다.

    > SELECT SupplierName
    >
    > FROM Suppliers
    >
    > WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.SupplierID AND Price = 22);

- **SQL ANY / ALL 연산자**

  - `ANY` 및 `ALL` 연산자를 사용하면 **하나의 컬럼 값을 다른 값 범위와 비교** 할 수 있다.

- **ANY 구문**

  - 결과로 `boolean` 값을 반환한다.
  - 하위 쿼리 값 중 **하나라도 조건을 충족하는 경우 `TRUE`**를 반환한다.

  > **SELECT column_name(s)** 
  >
  > **FROM** table_name
  >
  > **WHERE** column_name operator **ANY**
  >
  >  (**SELECT** column_name
  >
  >  **FROM** table_name
  >
  >  **WHERE** condition);

- ( 참고 )  연산자는 표준 비교 연산자 ( `=` , `<>` , `!=` , `>` , `>=` , `<` , `<=` ) 여야 한다.

- 다음 SQL 문은  `OrderDetails` 테이블에서 `Quantity` 가 10인 레코드를 찾으면 해당 `ProductID`의  `ProductName` 을 나열한다.

  > **SELECT** ProductName
  >
  > **FROM** Products
  >
  > **WHERE** ProductID = ANY
  >
  >  ( **SELECT** ProductID 
  >
  >    **FROM** OrderDetails
  >
  >   **WHERE** Quantity=10);

- **ALL 구문**

  - 결과로 `boolean` 값을 반환한다.

  - **모든 하위 쿼리 값이 조건을 충족하면 `TRUE`**를 반환한다.

  - `SELECT`,  `WHERE` 및 `HAVING` 문과 함께 사용된다.

  - **SELECT 구문에서의 ALL 사용법**

    > **SELECT ALL** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

  - 다음 SQL 문은 모든 제품 이름을 나열한다.

    > **SELECT ALL** ProductName
    >
    > **FROM** Products;
    >
    > **WHERE** TRUE;

  - **WHERE / HAVING 구문에서의  ALL 사용법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** column_name operator **ALL**
    >
    >  ( **SELECT** column_name
    >
    >    **FROM** table_name 
    >
    >    **WHERE** condition );

  - ( 참고 )  연산자는 표준 비교 연산자 ( `=` , `<>` , `!=` , `>` , `>=` , `<` , `<=` ) 여야 한다.

  - 다음 SQL 문은 `OrderDetails` 테이블의 모든 레코드에 `Quantity`가 10인 경우 `ProductName`을 나열한다. 해당 테이블에는 `Quantity` 열에 10 이외에 다양한 값이 있기 때문에 `FALSE`를 반환한다.

- **SQL SELECT INTO 문**

  - `SELECT INTO` 문은 한 테이블의 데이터를 새 테이블로 복사한다.

  - 다음 SQL 문은 모든 열을 새 테이블에 복사한다.

    > **SLEECT** *
    >
    > **INTO** newtable  [ **IN** externaldb ]
    >
    > **FROM** oldtable
    >
    > **WHERE** condition;

    

  - 다음 SQL 문은 일부 열을 새 테이블에 복사한다.

    > **SLEECT** column1, column2, column3, ...
    >
    > **INTO** newtable  [ **IN** externaldb ]
    >
    > **FROM** oldtable
    >
    > **WHERE** condition;

  - 새 테이블은 이전 테이블에 정의된 열 이름 및 유형으로 생성됩니다. `AS` 절을 사용하여 새 열 이름을 만들 수 있다.

  - 다음 SQL 문은 `Customers`의 백업 복사본을 생성한다.

    > **SELECT** * **INTO** CustomersBackUp2021
    >
    > **FROM** Customers;

  - 다음 SQL 문은  `IN` 절을 사용하여 테이블을 다른 데이터베이스의 새 테이블로 복사한다.

    > **SELECT** * **INTO** CustomersBackup2021 **IN** 'Backup.mdb'
    >
    > **FROM** Customers;

  - 다음 SQL 문은 몇 개의 열만 새 테이블에 복사한다.

    > **SELECT** CustomerName, ContactName **INTO** CustomersBackup2021
    >
    > **FROM** Customers;

  - 다음 SQL 문은 독일 고객만 새 테이블 복사한다.

    > **SELECT** * **INTO** CustomersGermany
    >
    > **FROM** Customers
    >
    > **WHERE** Country = 'Germany';

  - 다음 SQL 문은 두 개 이상의 테이블에서 새 테이블로 데이터를 복사한다.

    > **SELECT** C.CustomerName, O.OrderID
    >
    > **INTO** CustomersOrderBackup2021
    >
    > **FROM** Customers **AS** C
    >
    > **LEFT JOIN** Orders **AS** O **ON** C.CustomerID=O.CustomerID

  - ( 팁 ) `SELECT INTO`를 사용하여 다른 테이블의 스키마를 사용하여 비어 있는 새 테이블을 생성할 수도 있다. 쿼리가 데이터를 반환하지 않도록 하는 `WHERE` 절을 추가하기만 하면 된다. 

    > **SELECT** * **INTO** newtable
    >
    > **FROM** oldtable
    >
    > **WHERE** 1 = 0;

- **SQL INSERT INTO SELECT 문**

  - `INSERT INTO SELECT` 문은 **한 테이블의 데이터를 복사하여 다른 테이블에 삽입** 한다.

  - `INSERT INTO SELECT` 문을 사용하려면 **원본 및 대상 테이블의 데이터 형식이 일치** 해야 한다.

  - **INSERT INTO SELECT**

    - 한 테이블의 모든 열을 다른 테이블로 복사

      > **INSERT INTO** table2
      >
      > **SELECT** * **FROM** table1
      >
      > **WHERE** condition;

    - 한 테이블의 일부 열만 다른 테이블로 복사

      > **INSERT INTO** table2 (column1, column2, column3, ...)
      >
      > **SELECT** column1, column2, column3, ...
      >
      > **FROM** table1
      >
      > **WHERE** condition;

    - 다음 SQL 문은 `Suppliers`를  `Customers`로 복사한다. (데이터로 채워지지 않은 열에는 NULL이 포함된다)

      > **INSERT INTO** Customers (CustomerName, City, Country)
      >
      > **SELECT** SupplierName, Cit, Country 
      >
      > **FROM** Suppliers;

    - 다음 SQL 문은 `Suppliers`를  `Customers`로 복사한다. (모든 열을 채운다)

      > **INSERT INTO** Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
      >
      > **SELECT** SupplierName, ContactName, Address, City, PostalCode, Country 
      >
      > **FROM** Suppliers;

- **SQL CASE 문**

  - `CASE` 문은 조건을 통과하다가 **첫 번째 조건이 충족될 때** 값을 반환한다 (`if-then-else` 문과 비슷). 따라서 **조건이 `true`이면 읽기를 중지하고 결과를 반환** 한다. 조건이 참이 아니면 `ELSE`  절의 값을 반환한다.

  - `ELSE` 부분이 없고 조건이 참이면 `NULL`을 반환한다.

    > **CASE**
    >
    > 		**WHEN** condition1 **THEN** result1
    > 	
    > 		**WHEN** condition2 **THEN** result2
    > 	
    > 		**WHEN** conditionN **THEN** resultN
    > 	
    > 		**ELSE** result
    >
    > **END**;

  - 다음 SQL 문은  CASE 문에 지정된 `Quantity` 조건에 따라 각각 다른 `QuantityText` 값을 반환한다.

    > **SELECT** OrderID, Quantity
    >
    > **CASE**
    >
    > 		**WHEN** Quantity > 30 **THEN** 'The quantity is greater than 30'
    > 	
    > 		**WHEN** Quantity = 30 **THEN** 'The quantity is 30'
    > 	
    > 		**ELSE** 'The quantity is under 30'
    >
    > **END AS** QuantityText
    >
    > **FROM** OrderDetails;

  - 다음 SQL 문은 `City` 별로 고객을 정렬한다. 하지만 `City`가  `NULL`이면  `Country` 별로 정렬한다.

    > **SELECT** CustomerName, City, Country
    >
    > **FROM** Customers
    >
    > **ORDER BY**
    >
    > (**CASE**
    >
    > 		**WHEN** City **IS NULL THEN** Country
    > 	
    > 		**ELSE** City
    >
    > **END**);


# References

- [출처-1 : SQL Tutorial  ( w3schools.com )](https://www.w3schools.com/sql/default.asp) 
