# SQL

## SQL INSERT INTO SELECT 문

  - `INSERT INTO SELECT` 문은 **한 테이블의 데이터를 복사하여 다른 테이블에 삽입** 한다.

  - `INSERT INTO SELECT` 문을 사용하려면 **원본 및 대상 테이블의 데이터 형식이 일치** 해야 한다.

  - **INSERT INTO SELECT**

    - 한 테이블의 모든 열을 다른 테이블로 복사

      > **INSERT INTO** table2
      >
      > **SELECT** * **FROM** table1
      >
      > **WHERE** condition;

    - 한 테이블의 일부 열만 다른 테이블로 복사

      > **INSERT INTO** table2 (column1, column2, column3, ...)
      >
      > **SELECT** column1, column2, column3, ...
      >
      > **FROM** table1
      >
      > **WHERE** condition;

    - 다음 SQL 문은 `Suppliers`를  `Customers`로 복사한다. (데이터로 채워지지 않은 열에는 NULL이 포함된다)

      > **INSERT INTO** Customers (CustomerName, City, Country)
      >
      > **SELECT** SupplierName, Cit, Country 
      >
      > **FROM** Suppliers;

    - 다음 SQL 문은 `Suppliers`를  `Customers`로 복사한다. (모든 열을 채운다)

      > **INSERT INTO** Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
      >
      > **SELECT** SupplierName, ContactName, Address, City, PostalCode, Country 
      >
      > **FROM** Suppliers;


# References

- [SQL Insert Into Select - w3schools.com](https://www.w3schools.com/sql/sql_insert_into_select.asp) 
