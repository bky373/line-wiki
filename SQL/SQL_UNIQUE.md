# SQL

## SQL UNIQUE 제약 조건

  - UNIQUE 제약 조건은 **열의 모든 값이 서로 다른지 확인** 한다.

  - UNIQUE 및 PRIMARY KEY 제약 조건 모두 **컬럼 또는 컬럼 집합의 고유성** 을 보장한다.

  - **PRIMARY KEY 제약 조건에는 자동으로 UNIQUE 제약 조건이 있다.**

  - 테이블당 여러 UNIQUE 제약 조건을 가질 수 있지만, **PRIMARY KEY는 테이블당 오직 하나** 의 제약 조건만 가질 수 있다.

  - **CREATE TABLE에  UNIQUE 제약 조건 넣기**

    - 다음 SQL은 "Persons" 테이블이 생성될 때 "ID" 열에 대한 UNIQUE 제약 조건을 생성한다.

    - **SQL Server  /  Oracle  /  MS Access**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL UNIQUE**,
      > 	
      > 		LastName varchar(255) **NOT NULL**,
      > 	
      > 		FirstName varchar(255),
      > 	
      > 		Age int
      >
      > );

    - **MySQL**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL**,
      > 	
      > 		LastName varchar(255) **NOT NULL**,
      > 	
      > 		FirstName varchar(255),
      > 	
      > 		Age int,
      > 	
      > 		**UNIQUE** (ID)
      >
      > );

    - UNIQUE 제약 조건의 이름을 지정하고 여러 열에 대한 UNIQUE 제약 조건을 정의하려면 다음 SQL 구문을 사용한다.

    - **MySQL / SQL Server / Oracle / MS Access**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**CONSTRAINT** UC_Person **UNIQUE**  (ID, LastName)
      >
      > );

  - **ALTER TABLE에  UNIQUE 제약 조건 넣기**

    - **MySQL / SQL Server / Oracle / MS Access**

      > **ALTER TABLE** Persons
      >
      > **ADD UNIQUE** (ID);

  - UNIQUE 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 UNIQUE 제약 조건** 을 정의하려면 다음 SQL 구문을 사용한다.

    - **MySQL / SQL Server / Oracle / MS Access**

      > **ALTER TABLE** Persons
      >
      > **ADD CONSTRAINT** UC_Person **UNIQUE** (ID, LastName);

  - **UNIQUE 제약 조건 삭제**

    - **MySQL**

      > **ALTER TABLE** Persons
      >
      > **DROP INDEX** UC_Person;

    - **SQL Server  /  Oracle  /  MS Access**

      > **ALTER TABLE** Persons
      >
      > **DROP CONSTRAINT** UC_Person;

# References

- [SQL Unique - w3schools.com](https://www.w3schools.com/sql/sql_unique.asp) 
