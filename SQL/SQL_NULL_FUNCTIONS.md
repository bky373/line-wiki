# SQL

## SQL NULL 관련 함수

  - **`IFNULL()`, `ISNULL()`, `COALESCE()`, `NVL()` 함수**

    - `UnitsOnOrder` 열이 `optional` 하고, `NULL` 값을 포함할 수 있다고 가정한다.

    - `Products` 테이블

      ![image](https://user-images.githubusercontent.com/87057868/128313812-94078e4e-00bb-47ec-bf79-0cfcd6713ddd.png)

      > **SELECT** ProductName, UnitPrice * (UnitsInStock + UnitsOnOrder)
      >
      > **FROM** Products;

    - 위의 예에서 `UnitsOnOrder` 값 중 하나라도 `NULL`이면  결과는 `NULL`이 된다.

    - 위의 문제는 아래와 같은 방법으로 해결할 수 있다.

  - **MySQL**

    - `MySQL IFNULL()` 함수를 사용하면 표현식이 `NULL`인 경우 대체 값을 반환할 수 있다

    > **SELECT** ProductName, UnitPrice * (UnitsInStock + **IFNULL**(UnitsOnOrder, 0))
    >
    > **FROM** Products;

    > **SELECT** ProductName, UnitPrice * (UnitsInStock + **COALESCE**(UnitsOnOrder, 0))
    >
    > **FROM** Products;

  - **SQL Server**

    - `SQL Server ISNULL()` 함수를 사용하면 표현식이 NULL일 때 대체 값을 반환할 수 있다.

    > **SELECT** ProductName, UnitPrice * (UnitsInStock + **ISNULL**(UnitsOnOrder, 0))
    >
    > **FROM** Products;

  - **Oracle**

    - `Oracle NVL()` 함수는 위와 동일한 결과를 얻는다.

    > **SELECT** ProductName, UnitPrice * (UnitsInStock + **NVL**(UnitsOnOrder, 0))
    >
    > **FROM** Products;

  - **MS Access**

    - `SQL Server IsNULL()` 함수는 표현식이 `NULL`일 때 `TRUE(-1)` 을 반환하고, 그렇지 않으면  `FALSE(0)`을 반환한다.

    > **SELECT** ProductName, UnitPrice * (UnitsInStock + **IIF**(**IsNULL**(UnitsOnOrder), 0, UnitsOnOrder))
    >
    > **FROM** Products;


# References

- [SQL Null Functions - w3schools.com](https://www.w3schools.com/sql/sql_isnull.asp) 

