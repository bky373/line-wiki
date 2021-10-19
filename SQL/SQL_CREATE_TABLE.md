# SQL

## SQL CREATE TABLE 문

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
      
      
# References

- [SQL Create Table - w3schools.com](https://www.w3schools.com/sql/default.asp) 
