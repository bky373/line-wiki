# SQL

## SQL 제약 조건(Constraints)

  - **SQL 제약 조건** 은 테이블의 **데이터에 대한 규칙** 을 지정하는 데 사용된다.
  - 제약 조건은 테이블에 들어갈 수 있는 **데이터 유형을 제한** 하는 데 사용된다. 이렇게 하면 테이블에 있는 **데이터의 정확성과 신뢰성** 이 보장된다. 제약 조건과 데이터 작업 사이에 위반이 있으면 작업이 중단된다.
  - 제약 조건은 **컬럼 수준 또는 테이블 수준** 일 수 있다.

- **SQL 제약 조건 생성**

  - 제약 조건은 `CREATE TABLE` 문으로 테이블을 생성할 때 또는 `ALTER TABLE` 문으로 테이블을 생성한 후에 지정할 수 있다.

    > **CREATE TABLE** table_name (
    >
    > 		column1 datatype constraint,
    > 	
    > 		column2 datatype constraint,
    > 	
    > 		column3 datatype constraint,
    > 	
    > 		....
    >
    > );

  - 다음 제약 조건은 SQL에서 일반적으로 사용된다.

    - **NOT NULL** - 열이 NULL 값을 가질 수 없도록 한다.
    - **UNIQUE** - 열의 모든 값이 서로 다른지 확인한다.
    - **PRIMARY KEY** - NOT NULL과 UNIQUE의 조합. 테이블의 각 행을 고유하게 식별한다.
    - **FOREIGN KEY** - 테이블 간의 링크를 파괴하는 작업을 방지한다.
    - **CHECK** - 열의 값이 특정 조건을 충족하는지 확인한다.
    - **DEFAULT** - 값이 지정되지 않은 경우 열의 기본값을 설정한다.
    - **CREATE INDEX** - 데이터베이스에서 매우 빠르게 데이터를 생성하고 검색하는 데 사용된다.


# References

- [SQL Constraints - w3schools.com](https://www.w3schools.com/sql/sql_constraints.asp) 
