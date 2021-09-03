# SQL

## SQL HAVING 절

  - `WHERE` 키워드는 집계 함수와 함께 사용할 수 없기 때문에 `HAVING` 절이 SQL에 추가되었다.

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** condition
    >
    > **GROUP BY** column_name(s)
    >
    > **HAVING** condition
    >
    > **ORDER BY** column_name(s);

  - 다음 SQL 문은 각 국가의 고객의 수를 나열한다. 이때 고객이 5명 이상인 나라만 포함한다.

    > **SELECT COUNT**(CustomerID), Country
    >
    > **FROM** Customers
    >
    > **GROUP BY** Country
    >
    > **HAVING COUNT**(CustomerID) > 5;

  - 다음 SQL 문은 10개 이상의 주문을 등록한 직원을 주문 순서가 많은 순으로 나열한다.

    > **SELECT** E.LastName, **COUNT**(O.OrderID) AS NumberOfOrders
    >
    > **FROM** Orders **AS** O
    >
    > **INNER JOIN** Employees **AS** E 
    >
    > **ON** O.EmployeeID = E.EmployeeID
    >
    > **GROUP BY** LastName
    >
    > **HAVING COUNT**(O.OrderID) > 10
    >
    > **ORDER BY COUNT**(O.OrderID) **DESC;**

  - 다음 SQL 문은 직원 `"Davolio"` 또는 `"Fuller"` 가 25개 이상의 주문을 등록했는지 여부를 나열한다.

    > **SELECT** E.LastName, **COUNT(**O.OrderID) **AS** NumberOfOrders
    >
    > **FROM** Orders
    >
    > **INNER JOIN** Employees
    >
    > **ON** O.EmployeeID = E.EmployeeID
    >
    > **WHERE** LastName='Davolio' **OR** LastName='Fuller'
    >
    > **GROUP BY** LastName
    >
    > **HAVING COUNT**(O.OrderID) > 25;


# References

- [SQL Having - w3schools.com](https://www.w3schools.com/sql/sql_having.asp) 
