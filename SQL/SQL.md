# SQL

- **SQL PRIMARY KEY 제약 조건**

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

- **SQL FOREIGN KEY 제약 조건**

  - FOREIGN KEY 제약 조건은 **테이블 간의 링크를 파괴하는 작업을 방지** 하는 데 사용된다.

  - FOREIGN KEY는 **다른 테이블의 PRIMARY KEY를 참조하는 한 테이블의 필드(또는 필드 모음)** 이다.

  - 외래 키가 있는 테이블을 **자식 테이블** 이라고 하고, 기본 키가 있는 테이블을 **참조 또는 부모 테이블** 이라고 한다.

    ![image](https://user-images.githubusercontent.com/87057868/128487519-619f6187-30ef-4108-8e68-c318c01fa6d1.png)

    ![image](https://user-images.githubusercontent.com/87057868/128487606-bdfc1b7a-c49d-4d22-a06c-f1464f1677ee.png)

  - "Orders" 테이블의 "PersonID" 열은 "Persons" 테이블의 "PersonID" 열을 가리킨다.

  - "Persons" 테이블의 "PersonID" 열은 "Persons" 테이블의 PRIMARY KEY이다.

  - "Orders" 테이블의 "PersonID" 열은 "Orders" 테이블의 FOREIGN KEY이다.

  - FOREIGN KEY 제약 조건은 부모 테이블에 포함된 값 중 하나여야 하므로 외래 키 열에 잘못된 데이터가 삽입되는 것을 방지한다.

- **CREATE TABLE의 FOREIGN KEY**

  - 다음 SQL은 "Orders" 테이블이 생성될 때 "PersonID" 열에 FOREIGN KEY를 생성한다.

  - **MySQL**

    > **CREATE TABLE** Orders (
    >
    > 		OrderID int **NOT NULL**,	OrderNumber int **NOT NULL**,	PersonID int,	**PRIMARY KEY** (OrderID),	**FOREIGN KEY** (PersonID) **REFERENCES** (PersonID)
    >
    > );

  - **SQL Server  /  Oracle  /  MS Access**

    > **CREATE TABLE** Orders (
    >
    > 		OrderID int **NOT NULL PRIMARY KEY**,	OrderNumber int **NOT NULL**,	PersonID int **FOREIGN KEY REFERENCES** Person(PersonID)
    >
    > );

  - FOREIGN KEY 제약 조건의 **이름 지정을 허용** 하고 **여러 열에 대한 FOREIGN KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용하자.

    > **CREATE TABLE** Orders (
    >
    > 		OrderID int **NOT NULL**,	OrderNumber int **NOT NULL**,	PersonID int,	**PRIMARY KEY** (OrderID),	**CONSTRAINT** FK_PersonOrder **FOREIGN KEY** (PersonID)	**REFERENCES** Persons(PersonID)
    >
    > );

- **CREATE TABLE의 FOREIGN KEY**

  - "Orders" 테이블이 이미 생성된 경우 "PersonID" 열에 대한 FOREIGN KEY 제약 조건을 생성하려면 다음 SQL을 사용한다.

    > **ALTER TABLE** Orders
    >
    > **ADD FOREIGN KEY** (PersonID) **REFERENCES** Persons (PersonID);

  - FOREIGN KEY 제약 조건의 **이름 지정** 을 허용하고 **여러 열에 대한 FOREIGN KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용한다.

    > **ALTER TABLE** Orders
    >
    > **ADD CONSTRAINT** FK_PersonOrder
    >
    > **FOREIGN KEY** (PersonID) **REFERENCES** Persons(PersonID);

- **FOREIGN KEY 제약 조건 삭제**

  - **MySQL**

    > **ALTER TABLE** Orders
    >
    > **DROP FOREIGN KEY** FK_PersonOrder;

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Orders
    >
    > **DROP CONSTRAINT** FK_PersonOrder;

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
