# SQL

## SQL FOREIGN KEY 제약 조건

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


# References

- [SQL Foreign Key - w3schools.com](https://www.w3schools.com/sql/sql_foreignkey.asp) 
