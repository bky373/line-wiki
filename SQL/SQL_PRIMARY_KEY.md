# SQL

## SQL PRIMARY KEY 제약 조건

  - PRIMARY KEY 제약 조건은 테이블의 각 레코드를 고유하게 식별한다.
  - **기본 키는 UNIQUE 값을 포함해야 하며 NULL 값을 포함할 수 없다.**
  - 테이블에는 하나의 기본 키만 있을 수 있다. 테이블에서 이 **기본 키는 단일 또는 다중 열(필드)로 구성될 수 있다.**

- **CREATE TABLE의 SQL 기본 키**

  - 다음 SQL은 "Persons" 테이블이 생성될 때 "ID" 열에 PRIMARY KEY를 생성한다.

    - **MySQL**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**PRIMARY KEY** (ID)
      >
      > );

    - **SQL Server  /  Oracle  /  MS Access**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL PRIMARY KEY**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int
      >
      > );

  - PRIMARY KEY 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 PRIMARY KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용하자.

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**CONSTRAINT** PK_Person **PRIMARY KEY** (ID, LastName)
    >
    > );

  - 참고: 위의 예에는 하나의 PRIMARY KEY(PK_Person)만 있다. 그러나 기본 키의 VALUE는 2개의 COLUMNS(ID + LastName)로 구성되어 있다.

- **ALTER TABLE의 SQL 기본 키**

  > **ALTER TABLE** Persons
  >
  > **ADD PRIMARY KEY**  (ID);

  - PRIMARY KEY 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 PRIMARY KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용하자.

    > **ALTER TABLE** Persons
    >
    > **ADD CONSTRAINT** PK_Person **PRIMARY KEY** (ID, LastName);

  - 참고: ALTER TABLE을 사용하여 기본 키를 추가하는 경우 기본 키 열은 (테이블이 처음 생성되었을 때) NULL 값을 포함하지 않도록 선언해야 한다.

- **PRIMARY KEY 제약 조건 삭제**

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **DROP PRIMARY KEY**;

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Persons
    >
    > **DROM CONSTRAINT** PK_Person;


# References

- [SQL Primary Key - w3schools.com](https://www.w3schools.com/sql/sql_primarykey.asp) 

