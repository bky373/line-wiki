# SQL

## SQL DEFAULT 제약 조건

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

- [SQL Default - w3schools.com](https://www.w3schools.com/sql/sql_default.asp) 
