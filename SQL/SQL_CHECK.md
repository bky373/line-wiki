# SQL

## SQL CHECK 제약 조건

  - CHECK 제약 조건은 컬럼에 넣을 수 있는 **값의 범위를 제한** 하기 위해 사용한다.
  - 열에 CHECK 제약 조건을 정의하면 이 열에 대해 특정 값만 허용된다.
  - 테이블에 CHECK 제약 조건을 정의하면 행의 다른 컬럼의 값을 기반으로 특정 컬럼의 값을 제한할 수 있다.

- **CREATE TABLE에  CHECK 제약 조건 넣기**

  - 다음 SQL은 "Person" 테이블이 생성될 때 "Age" 열에 대한 CHECK 제약 조건을 생성한다. CHECK 제약 조건은 사람의 연령이 18세 이상이어야 함을 보장한다.

  - **MySQL**

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**CHECK** (Age>=18)
    >
    > );

  - **SQL Server  /  Oracle  /  MS Access**

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int **CHECK** (Age >= 18)
    >
    > );

  - CHECK 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 CHECK 제약 조건을 정의** 하려면 다음 SQL 구문을 사용한다.

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
    > 		City varchar(255),
    > 	
    > 		**CONSTRAINT** CHK_Person **CHECK** (Age>=18 **AND** City='Sandnes')
    >
    > );

- **ALTER TABLE에  CHECK 제약 조건 넣기**

  > **ALTER TABLE** Persons
  >
  > **ADD CHECK** (Age >= 18);

  - CHECK 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 CHECK 제약 조건을 정의** 하려면 다음 SQL 구문을 사용한다.

    > **ALTER TABLE** Persons
    >
    > **ADD CONSTRAINT** CHK_PersonAge **CHECK** (Age>=18 **AND** City='Sandnes');

- **CHECK 제약 조건 삭제**

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Persons
    >
    > **DROP CONSTRAINT** CHK_PersonAge;

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **DROP CHECK** CHK_PersonAge;


