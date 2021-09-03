# SQL

## **SQL EXISTS 연산자**

  - `EXISTS` 연산자는 **하위 쿼리에 레코드가 있는지** 테스트하는 데 사용된다.

  - `EXISTS` 연산자는 하위 쿼리가 **하나 이상의 레코드** 를 반환하는 경우 **`TRUE`를 반환** 한다.

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE EXISTS**
    >
    > (**SELECT** column_name **FROM** table_name **WHERE** condition);

  - 다음 SQL 문은 `TRUE` 를 반환하고 제품 가격이 20 미만인 공급자를 나열한다.

    > **SELECT** SupplierName
    >
    > **FROM** Suppliers
    >
    > **WHERE EXISTS** (**SELECT** ProductName **FROM** Products **WHERE** Products.SupplierID = Suppliers.supplier **AND** Price < 20)

  - 다음 SQL 문은 `TRUE`를 반환하고 제품 가격이 22인 공급자를 나열한다.

    > SELECT SupplierName
    >
    > FROM Suppliers
    >
    > WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.SupplierID AND Price = 22);


# References

- [SQL Exists - w3schools.com](https://www.w3schools.com/sql/sql_exists.asp) 
