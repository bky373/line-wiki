# SQL

## SELF JOIN

  - `SELF JOIN` 은 이름에서처럼 일반적인 조인의 기능을 수행하지만, 테이블 자신을 조인한다는 점에서 다른 조인과 다르다.

  - 아래 SQL 문에서 `T1`과  `T2`는 같은 테이블을 각각 다른 별칭으로 지정한 것이다.

    > **SELECT** column_name(s)
    >
    > **FROM** table1 T1,  table2 T2
    >
    > **WHERE** condition;
    >
    > (실사용 예시)
    >
    > **SELECT** A.City, A.CustomerName **AS** CustomerName1, B.CustomerName **AS** CustomerName2
    >
    > **FROM** Customers A, Customers B
    >
    > **WHERE** A.CustomerID <> B.CustomerID
    >
    > **AND** A.City = B.City
    >
    > **ORDER BY** A.City;

  - 위의 SQL 문은 같은 도시에 있는 다른 고객들을 (도시를 기준으로) 결합시켜 이름을 나타낸 것이다.

    ![image](https://user-images.githubusercontent.com/49539592/128044922-3233b8ed-4a77-4db5-9e32-10ba8cc2a3dc.png)
    
    
# References

- [SQL Self Join - w3schools.com](https://www.w3schools.com/sql/sql_join_self.asp) 
