# SQL

## INNER JOIN 키워드

  - `INNER JOIN` 키워드는 두 테이블에서 일치하는 값을 가진 레코드를 선택한다.

    > **SELECT** column_name(s)
    > **FROM** table1
    > **INNER JOIN** table2
    > **ON** table1.column_name = table2.column_name;
    >
    > (실사용 예시)
    >
    > **SELECT** Orders.OrderID, Customers.CustomerName
    > **FROM** Orders
    > **INNER JOIN** Customers 
    >
    > **ON** Orders.CustomerID = Customers.CustomerID;

  - 다음 SQL 문은 세 개의 테이블을 조인한다.

    > **SELECT** Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
    > **FROM** ( (Orders
    > **INNER JOIN** Customers 
    >
    > **ON** Orders.CustomerID = Customers.CustomerID)
    > **INNER JOIN** Shippers 
    >
    > **ON** Orders.ShipperID = Shippers.ShipperID );


# References

- [SQL Inner Join - w3schools.com](https://www.w3schools.com/sql/sql_join_inner.asp) 
