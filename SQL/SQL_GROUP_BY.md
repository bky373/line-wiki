# SQL

## GROUP BY 문

  - `GROUP BY` 문은  `"각 국가의 고객 수 찾기"` 와 같이 **동일한 값을 가진 행을 요약 행으로 그룹화** 한다. 
  - `GROUP BY` 문은 하나 이상의 열로 결과 집합을 그룹화하기 위해 **집계 함수** ( `COUNT()`, `MAX()`, `MIN()`, `SUM()`, `AVG()` ) 와 함께 자주 사용된다.

  > **SELECT** column_name(s)
  >
  > **FROM** table_name
  >
  > **WHERE** condition
  >
  > **GROUP BY** column_name(s)
  >
  > **ORDER BY** column_name(s);
  >
  > (실사용 예시)
  >
  > **SELECT** S.ShipperName, **COUNT**(O.OrderID) **AS** NumberOfOrders 
  >
  > **FROM** Orders **AS** O
  >
  > **LEFT JOIN** Shippers **AS** S **ON** O.ShipperID=S.ShipperID
  >
  > **GROUP BY** ShipperName;

# References

- [SQL Group By - w3schools.com](https://www.w3schools.com/sql/sql_groupby.asp) 
