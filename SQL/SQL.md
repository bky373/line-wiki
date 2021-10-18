# SQL

- **SQL CREATE TABLE 문**

  - `CREATE TABLE` 문은 데이터베이스에 새 테이블을 만드는 데 사용된다.

    > **CREATE TABLE** table_name (
    >
    > 		column1 datatype,
    > 	
    > 		column2 datatype,
    > 	
    > 		column3 datatype,
    > 	
    > 		...
    >
    > );

    - 컬럼 매개변수는 테이블의 컬럼 이름을 지정한다.
    - `datatype` 매개변수는 컬럼이 보유할 수 있는 데이터 유형(예: varchar, 정수, 날짜 등)을 지정한다.
    - 팁: 사용 가능한 데이터 유형에 대한 개요를 보려면 전체 [데이터 유형 참조](https://www.w3schools.com/sql/sql_datatypes.asp)하자.

  - 다음 예에서는 `PersonID`, `LastName`, `FirstName`, `Address` 및 `City`의 5개 열이 포함된 `"Persons"`라는 테이블을 만든다.

    > **CREATE TABLE** Persons (
    >
    > 		PersonID int,
    > 	
    > 		LastName varchar(255),
    > 	
    > 		FirstName varchar(255),
    > 	
    > 		Address varchar(255),
    > 	
    > 		City varchar(255)
    >
    > );

  - 이제 비어 있는 `"Persons"` 테이블이 다음과 같이 표시된다.

    ![image](https://user-images.githubusercontent.com/49539592/128461281-c8a449b2-1f1a-4d26-bf4c-b8b1055c840c.png)

  - 팁: 이제 `INSERT INTO` 문을 사용하여 빈 `"Persons"` 테이블을 데이터로 채울 수 있다.

  - `CREATE TABLE`을 사용하여 기존 테이블의 복사본을 만들 수도 있다.

    - 새 테이블은 동일한 컬럼 정의를 가져온다. 모든 컬럼을 가져오거나 또는 특정 컬럼을 가져올 수 있다.

    - 기존 테이블을 사용하여 새 테이블을 만드는 경우 새 테이블은 이전 테이블의 기존 값으로 채워진다.

      > **CREATE TABLE** new_table_name **AS**
      >
      > 		**SELECT** column1, column2, ...
      > 	
      > 		**FROM** existing_table_name
      > 	
      > 		**WHERE** .... ;

    - 다음 SQL은 `"TestTable"`(`"Customers"` 테이블의 복사본)라는 새 테이블을 만든다.

      > **CREATE TABLE** TestTable **AS**
      >
      > 		**SELECT** customername, contactname
      > 	
      > 		**FROM** Customers;

- **SQL DROP TABLE 문**

  - `DROP TABLE` 문은 데이터베이스의 기존 테이블을 삭제하는 데 사용된다.

    > **DROP TABLE** table_name;

  - 참고: 테이블을 떨어뜨리기 전에 주의하자. 테이블을 삭제하면 테이블에 저장된 모든 정보가 손실된다!

  - 다음 SQL 문은 기존 테이블 "Shippers"를 삭제한다.

    > **DROP TABLE** Shippers;

- **SQL TRUNCATE TABLE 문**

  - `TRUNCATE TABLE` 문은 테이블 내부의 데이터를 삭제하는 데 사용되지만 테이블 자체는 삭제하지 않는다.

    > **TRUNCATE TABLE** table_name;

- **SQL ALTER TABLE 문**

  - `ALTER TABLE` 문은 **기존 테이블의 열을 추가, 삭제 또는 수정** 하는 데 사용된다.
  - `ALTER TABLE` 문은 또한 기존 테이블에 **다양한 제약 조건을 추가 및 삭제** 하는 데 사용된다.

- **ALTER TABLE - 컬럼 추가**

  > **ALTER TABLE** table_name
  >
  > **ADD** column_name datatype;

  - 다음 SQL은 `"Customers"` 테이블에 `"Email"` 열을 추가한다.

  - 새로 만들어진 컬럼의 값은 `null`로 채워진다.

    > **ALTER TABLE** table_name
    >
    > **ADD** Email varchar (255);

- **ALTER TABLE - 컬럼 삭제**

  > **ALTER TABLE** table_name
  >
  > **DROP COLUMN** column_name;

  - 다음 SQL은 `"Customers"` 테이블에서 `"Email"` 열을 삭제한다.

    > **ALTER TABLE** Customers
    >
    > **DROP COLUMN** Email;

- **ALTER TABLE - 컬럼 변경/수정**

  - **SQL Server  /  MS Access**

    > **ALTER TABLE** table_name
    >
    > **ALTER COLUMN** column_name datatype;

  - **My SQL  /  Oracle  (10G 이전 버전)**

    > **ALTER TABLE** table_name
    >
    > **MODIFY COLUMN** column_name datatype;

  - **Oracle  (10G 버전 이후)**

    > **ALTER TABLE** table_name
    >
    > **MODIFY** column_name datatype;

- **SQL 제약 조건(Constraints)**

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

- **SQL NOT NULL 제약 조건**

  - 기본적으로 열은 NULL 값을 보유할 수 있다. NOT NULL 제약 조건은 열이 NULL 값을 허용하지 않도록 한다.

  - 이렇게 하면 필드에 항상 값이 포함된다. 즉, 이 필드에 값을 추가하지 않고는 새 레코드를 삽입하거나 레코드를 업데이트할 수 없다.

  - **CREATE TABLE에서  NOT NULL 제약 조건 넣기**

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar (255) **NOT NULL**,	FirstName varchar (255) **NOT NULL**,	Age int
    >
    > );

  - **ALTER TABLE에서  NOT NULL 제약 조건 넣기**

    > **ALTER TABLE** Persons
    >
    > **MODIFY** Age int **NOT NULL**;

- **SQL UNIQUE 제약 조건**

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

- [SQL Tutorial - w3schools.com](https://www.w3schools.com/sql/default.asp) 

