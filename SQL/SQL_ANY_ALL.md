# SQL

## SQL ANY / ALL 연산자

  - `ANY` 및 `ALL` 연산자를 사용하면 **하나의 컬럼 값을 다른 값 범위와 비교** 할 수 있다.

- **ANY 구문**

  - 결과로 `boolean` 값을 반환한다.
  - 하위 쿼리 값 중 **하나라도 조건을 충족하는 경우 `TRUE`**를 반환한다.

  > **SELECT column_name(s)** 
  >
  > **FROM** table_name
  >
  > **WHERE** column_name operator **ANY**
  >
  > (**SELECT** column_name
  >
  > **FROM** table_name
  >
  > **WHERE** condition);

- ( 참고 )  연산자는 표준 비교 연산자 ( `=` , `<>` , `!=` , `>` , `>=` , `<` , `<=` ) 여야 한다.

- 다음 SQL 문은  `OrderDetails` 테이블에서 `Quantity` 가 10인 레코드를 찾으면 해당 `ProductID`의  `ProductName` 을 나열한다.

  > **SELECT** ProductName
  >
  > **FROM** Products
  >
  > **WHERE** ProductID = ANY
  >
  > ( **SELECT** ProductID 
  >
  > **FROM** OrderDetails
  >
  > **WHERE** Quantity=10);

- **ALL 구문**

  - 결과로 `boolean` 값을 반환한다.

  - **모든 하위 쿼리 값이 조건을 충족하면 `TRUE`**를 반환한다.

  - `SELECT`,  `WHERE` 및 `HAVING` 문과 함께 사용된다.

  - **SELECT 구문에서의 ALL 사용법**

    > **SELECT ALL** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** condition;

  - 다음 SQL 문은 모든 제품 이름을 나열한다.

    > **SELECT ALL** ProductName
    >
    > **FROM** Products;
    >
    > **WHERE** TRUE;

  - **WHERE / HAVING 구문에서의  ALL 사용법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** column_name operator **ALL**
    >
    > ( **SELECT** column_name
    >
    > **FROM** table_name 
    >
    > **WHERE** condition );

  - ( 참고 )  연산자는 표준 비교 연산자 ( `=` , `<>` , `!=` , `>` , `>=` , `<` , `<=` ) 여야 한다.

  - 다음 SQL 문은 `OrderDetails` 테이블의 모든 레코드에 `Quantity`가 10인 경우 `ProductName`을 나열한다. 해당 테이블에는 `Quantity` 열에 10 이외에 다양한 값이 있기 때문에 `FALSE`를 반환한다.


# References

- [SQL Any, All - w3schools.com](https://www.w3schools.com/sql/sql_any_all.asp) 

