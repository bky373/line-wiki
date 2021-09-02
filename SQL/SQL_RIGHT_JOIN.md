# SQL

## RIGHT (OUTER) JOIN 키워드

  - `RIGHT JOIN` 키워드는 오른쪽 테이블(table2)의 모든 레코드와 왼쪽 테이블(table1)의 일치하는 레코드를 반환한다. 

  - 일치하는 항목이 없는 경우 왼쪽으로부터는 아무 레코드도 결합하지 않는다. ( 그래도 굳이 왼쪽 테이블의 컬럼을 표시한다면 해당 컬럼의 레코드는 `null` 값을 갖는다 )  

    > **SELECT** column_name(s)
    > **FROM** table1
    > **RIGHT JOIN** table2
    > **ON** table1.column_name = table2.column_name;
    >
    > (실사용 예시)
    >
    > **SELECT** Orders.OrderID, Employees.LastName, Employees.FirstName
    >
    > **FROM** Orders
    >
    > **RIGHT JOIN** Employees 
    >
    > **ON** Orders.EmployeeID = Employees.EmployeeID
    >
    > **ORDER BY** Orders.OrderID;

# References

- [SQL Right Join - w3schools.com](https://www.w3schools.com/sql/sql_join_right.asp) 
