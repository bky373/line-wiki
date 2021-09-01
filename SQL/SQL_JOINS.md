# SQL

## SQL JOINS

  - `JOIN` 절은 두 개 이상의 테이블에서, 서로 연결된 열에 근거하여 관련된 행들을 결합하기 위해 사용한다.

  - 예를 들어, 아래 두 가지 테이블 및 컬럼 정보가 있다.

    - `Orders` 테이블의 컬럼 : `OrderID`,  `CustomerID`,  `OrderDate`

      ![image](https://user-images.githubusercontent.com/49539592/128020423-72fab655-825c-4824-9dca-f95075ec8bc9.png)

    - `Customers` 테이블의 컬럼 : `CustomerID`,  `CustomerName`,  `ContactName`,  `Country`

      ![image](https://user-images.githubusercontent.com/49539592/128021072-6a88ba40-9982-4f6b-b238-0b140cd0f7d4.png)

  - 여기에서 `Orders` 테이블의 `CustomerID` 열은,  `Customers` 테이블의 `CustomerID`를 참조한다. 따라서 위의 두 테이블에서 `CustomerID` 열은 서로 관계가 있다고 할 수 있다.

  - 그렇다면, 다음 SQL 문 ( `INNER JOIN` )에서는, `CustomerID` 열을 기준으로 두 테이블에서 일치하는 레코드 값을 얻을 수 있다.

    > **SELECT** O.OrderID, C.CustomerName, O.OrderDate
    >
    > **FROM** Orders **AS** O
    >
    > **INNER JOIN** Customers **AS** C 
    >
    > **ON** O.CustomerID=C.CustomerID;

    ![image](https://user-images.githubusercontent.com/49539592/128021005-99b67201-f350-460d-a69b-a55781d9db3b.png)

- **SQL JOIN의 다양한 타입**

  - `(INNER) JOIN` : 두 테이블에서 **일치하는 값** 을 가진 레코드를 반환한다.

    > 결과적으로 **왼쪽 테이블과 오른쪽 테이블의 접점** 을 가진 레코드들을 결합하여 반환한다.

  - `LEFT (OUTER) JOIN` : **왼쪽 테이블의 모든 레코드** 를 반환하고, 왼쪽 테이블 컬럼의 값과 일치하는 **오른쪽 테이블의 레코드를 결합** 하여 반환한다.

    > - 왼쪽 테이블 컬럼의 값 하나가 있고, 이 값과 일치하는 오른쪽 테이블 컬럼의 레코드가 여러 개 있을 경우, 오른쪽 테이블 레코드의 수만큼 행이 만들어진다.
    >
    > - 만약 오른쪽 테이블에 일치하는 항목이 없다면 왼쪽 테이블의 레코드의 수만큼 행을 반환한다.

  - `RIGHT (OUTER) JOIN` : **오른쪽 테이블의 모든 레코드** 를 반환하고, 왼쪽 테이블에서는 오른쪽 테이블 컬럼의 값과 일치하는 레코드를 결합하여 반환한다.

    > - 오른쪽 테이블 컬럼의 값 하나가 있고, 이 값과 일치하는 왼쪽 테이블 컬럼의 레코드가 여러 개 있을 경우, 왼쪽 테이블 레코드의 수만큼 행이 만들어진다.
    >
    > - 만약 왼쪽 테이블에 일치하는 항목이 없다면 오른쪽 테이블의 레코드의 수만큼 행을 반환한다.

  - `FULL (OUTER) JOIN` : **왼쪽 또는 오른쪽 테이블** 에 일치하는 항목이 있는 경우 서로 결합하여 모든 레코드를 반환한다.

    > 결과적으로 **왼쪽 테이블과 오른쪽 테이블에서 일치하는 값들을 모두 결합시킨 레코드** 를 반환한다.

  ![image](https://user-images.githubusercontent.com/49539592/128021848-cad68bc8-6ceb-405c-ba4d-3458351518c6.png)


# References

- [SQL Joins - w3schools.com](https://www.w3schools.com/sql/sql_join.asp) 
