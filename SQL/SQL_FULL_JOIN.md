# SQL

## FULL (OUTER) JOIN 키워드

  - `FULL OUTER JOIN` 키워드는 왼쪽(table1) 또는 오른쪽(table2) 테이블 레코드에 일치 항목이 있는 경우 모든 레코드를 반환한다.

  - ( 주의! ) `FULL OUTER JOIN`은 잠재적으로 매우 거대한 레코드 집합을 결과값으로 반환할 수 있다.

    > **SELECT** column_name(s)
    >
    > **FROM** table1
    >
    > **FULL OUTER JOIN** table2
    >
    > **ON** table1.column_name = table2.column_name
    >
    > **WHERE** condition;
    >
    > (실사용 예시)
    >
    > **SELECT** Customers.CustomerName, Orders.OrderID
    >
    > **FROM** Customers
    >
    > **FULL OUTER JOIN** Orders 
    >
    > **ON** Customers.CustomerID=Orders.CustomerID
    >
    > **ORDER BY** Customers.CustomerName;

# References

- [SQL Full Join - w3schools.com](https://www.w3schools.com/sql/sql_join_full.asp) 
