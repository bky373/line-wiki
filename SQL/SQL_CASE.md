# SQL

## SQL CASE 문

  - `CASE` 문은 조건을 통과하다가 **첫 번째 조건이 충족될 때** 값을 반환한다 (`if-then-else` 문과 비슷). 따라서 **조건이 `true`이면 읽기를 중지하고 결과를 반환** 한다. 조건이 참이 아니면 `ELSE`  절의 값을 반환한다.

  - `ELSE` 부분이 없고 조건이 참이면 `NULL`을 반환한다.

    > **CASE**
    >
    > 		**WHEN** condition1 **THEN** result1
    > 	
    > 		**WHEN** condition2 **THEN** result2
    > 	
    > 		**WHEN** conditionN **THEN** resultN
    > 	
    > 		**ELSE** result
    >
    > **END**;

  - 다음 SQL 문은  CASE 문에 지정된 `Quantity` 조건에 따라 각각 다른 `QuantityText` 값을 반환한다.

    > **SELECT** OrderID, Quantity
    >
    > **CASE**
    >
    > 		**WHEN** Quantity > 30 **THEN** 'The quantity is greater than 30'
    > 	
    > 		**WHEN** Quantity = 30 **THEN** 'The quantity is 30'
    > 	
    > 		**ELSE** 'The quantity is under 30'
    >
    > **END AS** QuantityText
    >
    > **FROM** OrderDetails;

  - 다음 SQL 문은 `City` 별로 고객을 정렬한다. 하지만 `City`가  `NULL`이면  `Country` 별로 정렬한다.

    > **SELECT** CustomerName, City, Country
    >
    > **FROM** Customers
    >
    > **ORDER BY**
    >
    > (**CASE**
    >
    > 		**WHEN** City **IS NULL THEN** Country
    > 	
    > 		**ELSE** City
    >
    > **END**);


# References

- [SQL Case - w3schools.com](https://www.w3schools.com/sql/sql_case.asp) 
