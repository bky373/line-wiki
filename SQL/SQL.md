# SQL

- **SQL CHECK 제약 조건**

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

- **SQL DEFAULT 제약 조건**

  - DEFAULT 제약 조건은 컬럼의 기본값을 설정하는 데 사용된다.
  - 다른 값을 지정하지 않으면 기본값이 모든 새 레코드에 추가된다.

- **CREATE TABLE에 DEFAULT 넣기**

  - 다음 SQL은 "Persons" 테이블이 생성될 때 "City" 열에 대해 DEFAULT 값을 설정한다.

    > **CREATE TABLE** Persons (
    >
    > 		**ID** int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	City varchar(255) **DEFAULT** 'Sandnes'
    >
    > );

  - DEFAULT 제약 조건은 GETDATE()와 같은 함수를 사용하여 시스템 값을 삽입하는 데에도 사용할 수 있다.

    > **CREATE TABLE** Orders (
    >
    > 		ID int **NOT NULL**,	OrderNumber int **NOT NULL**,	OrderDate date **DEFAULT** GETDATE()
    >
    > );

- **ALTER TABLE에 DEFAULT 넣기**

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **ALTER** City **SET DEFAULT** 'Sandnes';

  - **SQL Server**

    > **ALTER TABLE** Persons
    >
    > **ADD CONSTRAINT** df_City
    >
    > **DEFAULT** 'Sandnes' FOR City;

  - **MS Access**

    > **ALTER TABLE** Persons
    >
    > **ALTER COLUMN** City **SET DEFAULT** 'Sandnes';

  - **Oracle**

    > **ALTER TABLE** Persons
    >
    > **MODIFY** City **DEFAULT** 'Sandnes';

- **DEFAULT 제약 조건 삭제**

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **ALTER** City **DROP DEFAULT**;

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Persons
    >
    > **ALTER COLUMN** City **DROP DEFAULT**;


# References

- [SQL Tutorial - w3schools.com](https://www.w3schools.com/sql/sql_create_table.asp) 
