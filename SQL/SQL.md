# SQL

- **LEFT (OUTER) JOIN 키워드**

  - `LEFT JOIN` 키워드는 왼쪽 테이블(table1)의 모든 레코드와 오른쪽 테이블(table2)의 일치하는 레코드를 반환한다. 

  - 일치하는 항목이 없는 경우 오른쪽으로부터는 아무 레코드도 결합하지 않는다. ( 그래도 굳이 오른쪽 테이블의 컬럼을 표시한다면 해당 컬럼의 레코드는 `null` 값을 갖는다 )

    > **SELECT** column_name(s)
    >
    > **FROM** table1
    >
    > **LEFT JOIN** table2
    >
    > **ON** table1.column_name = table2.column_name;
    >
    > (실사용 예시)
    >
    > **SELECT** Customers.CustomerName, Orders.OrderID
    > **FROM** Customers
    > **LEFT JOIN** Orders
    > **ON** Customers.CustomerID=Orders.CustomerID
    > **ORDER BY** Customers.CustomerName;

    ![image](https://user-images.githubusercontent.com/49539592/128034193-da548c93-8662-4d38-a1f7-de7cfc985291.png)

- **RIGHT (OUTER) JOIN 키워드**

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

- **FULL (OUTER) JOIN 키워드**

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

- **SQL SELF JOIN**

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

- **SQL UNION 연산자**

  - `UNION` 연산자는 **두 개 이상의 `SELECT` 문의 결과 집합을 결합** 하는 데 사용된다.

    - `UNION` 내의 모든 `SELECT` 문에는 **동일한 수의 컬럼** 이 있어야 한다. 
    - 컬럼끼리는 **유사한 데이터 타입** 을 가지고 있어야 한다. 
    - 모든 `SELECT` 문의 컬럼은 **같은 순서** 로 나열되어야 한다.

    > **SELECT** column_name(s) **FROM** table1
    >
    > **UNION**
    >
    > **SELECT** column_name(S) **FROM** table2;

- **UNION ALL 구문**

  - `UNION` 연산자는 기본적으로 **고유한** ( `distinct` )  값만 선택한다. 

  - **중복 값** 을 허용하려면 `UNION ALL` 을 사용해야 한다.

    > **SELECT** column_name(s) **FROM** table1
    >
    > **UNION ALL**
    >
    > **SELECT** column_name(s) **FROM** table2;

  - ( 주의! ) 결과 집합의 열 이름은 일반적으로 첫 번째 `SELECT` 문의 열 이름과 같다.

  - 다음 SQL 문은 모든 고객과 공급업체를 나열한다.

    > **SELECT** 'Customer' **AS** Type, ContactName, City, Country
    >
    > **FROM** Customers
    >
    > **UNION**
    >
    > **SELECT** 'Supplier', ContactName, City, Country
    >
    > **FROM** Suppliers;

    ![image](https://user-images.githubusercontent.com/87057868/128156725-b7e91501-ff3a-4038-b4fa-4ad900107c62.png)

- **GROUP BY 문**

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

- **SQL HAVING 절**

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

- **SQL EXISTS 연산자**

  - `EXISTS` 연산자는 **하위 쿼리에 레코드가 있는지** 테스트하는 데 사용된다.

  - `EXISTS` 연산자는 하위 쿼리가 **하나 이상의 레코드** 를 반환하는 경우 **`TRUE`를 반환** 한다.

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE EXISTS**
    >
    > (**SELECT** column_name **FROM** table_name **WHERE** condition);

  - 다음 SQL 문은 `TRUE` 를 반환하고 제품 가격이 20 미만인 공급자를 나열한다.

    > **SELECT** SupplierName
    >
    > **FROM** Suppliers
    >
    > **WHERE EXISTS** (**SELECT** ProductName **FROM** Products **WHERE** Products.SupplierID = Suppliers.supplier **AND** Price < 20)

  - 다음 SQL 문은 `TRUE`를 반환하고 제품 가격이 22인 공급자를 나열한다.

    > SELECT SupplierName
    >
    > FROM Suppliers
    >
    > WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.SupplierID AND Price = 22);

- **SQL ANY / ALL 연산자**

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

- **SQL SELECT INTO 문**

  - `SELECT INTO` 문은 한 테이블의 데이터를 새 테이블로 복사한다.

  - 다음 SQL 문은 모든 열을 새 테이블에 복사한다.

    > **SLEECT** *
    >
    > **INTO** newtable  [ **IN** externaldb ]
    >
    > **FROM** oldtable
    >
    > **WHERE** condition;

    

  - 다음 SQL 문은 일부 열을 새 테이블에 복사한다.

    > **SLEECT** column1, column2, column3, ...
    >
    > **INTO** newtable  [ **IN** externaldb ]
    >
    > **FROM** oldtable
    >
    > **WHERE** condition;

  - 새 테이블은 이전 테이블에 정의된 열 이름 및 유형으로 생성됩니다. `AS` 절을 사용하여 새 열 이름을 만들 수 있다.

  - 다음 SQL 문은 `Customers`의 백업 복사본을 생성한다.

    > **SELECT** * **INTO** CustomersBackUp2021
    >
    > **FROM** Customers;

  - 다음 SQL 문은  `IN` 절을 사용하여 테이블을 다른 데이터베이스의 새 테이블로 복사한다.

    > **SELECT** * **INTO** CustomersBackup2021 **IN** 'Backup.mdb'
    >
    > **FROM** Customers;

  - 다음 SQL 문은 몇 개의 열만 새 테이블에 복사한다.

    > **SELECT** CustomerName, ContactName **INTO** CustomersBackup2021
    >
    > **FROM** Customers;

  - 다음 SQL 문은 독일 고객만 새 테이블 복사한다.

    > **SELECT** * **INTO** CustomersGermany
    >
    > **FROM** Customers
    >
    > **WHERE** Country = 'Germany';

  - 다음 SQL 문은 두 개 이상의 테이블에서 새 테이블로 데이터를 복사한다.

    > **SELECT** C.CustomerName, O.OrderID
    >
    > **INTO** CustomersOrderBackup2021
    >
    > **FROM** Customers **AS** C
    >
    > **LEFT JOIN** Orders **AS** O **ON** C.CustomerID=O.CustomerID

  - ( 팁 ) `SELECT INTO`를 사용하여 다른 테이블의 스키마를 사용하여 비어 있는 새 테이블을 생성할 수도 있다. 쿼리가 데이터를 반환하지 않도록 하는 `WHERE` 절을 추가하기만 하면 된다. 

    > **SELECT** * **INTO** newtable
    >
    > **FROM** oldtable
    >
    > **WHERE** 1 = 0;

- **SQL INSERT INTO SELECT 문**

  - `INSERT INTO SELECT` 문은 **한 테이블의 데이터를 복사하여 다른 테이블에 삽입** 한다.

  - `INSERT INTO SELECT` 문을 사용하려면 **원본 및 대상 테이블의 데이터 형식이 일치** 해야 한다.

  - **INSERT INTO SELECT**

    - 한 테이블의 모든 열을 다른 테이블로 복사

      > **INSERT INTO** table2
      >
      > **SELECT** * **FROM** table1
      >
      > **WHERE** condition;

    - 한 테이블의 일부 열만 다른 테이블로 복사

      > **INSERT INTO** table2 (column1, column2, column3, ...)
      >
      > **SELECT** column1, column2, column3, ...
      >
      > **FROM** table1
      >
      > **WHERE** condition;

    - 다음 SQL 문은 `Suppliers`를  `Customers`로 복사한다. (데이터로 채워지지 않은 열에는 NULL이 포함된다)

      > **INSERT INTO** Customers (CustomerName, City, Country)
      >
      > **SELECT** SupplierName, Cit, Country 
      >
      > **FROM** Suppliers;

    - 다음 SQL 문은 `Suppliers`를  `Customers`로 복사한다. (모든 열을 채운다)

      > **INSERT INTO** Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
      >
      > **SELECT** SupplierName, ContactName, Address, City, PostalCode, Country 
      >
      > **FROM** Suppliers;

- **SQL CASE 문**

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

- **SQL NULL 관련 함수**

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

**SQL COMMENTS**

- **주석** 은 SQL 문의 섹션을 설명하거나 SQL 문의 실행을 방지하는 데 사용된다.

- **한 줄 주석** ( Single Line Comments )

  - 한 줄 주석은 `--`로 시작한다.

  - `--` 와 줄 끝 사이의 모든 텍스트는 무시된다 (실행되지 않는다).

    > --Select all:
    > **SELECT** * **FROM** Customers;

  - 다음 예제에서는 한 줄 주석을 사용하여 줄 끝을 무시한다.

    > **SELECT** * **FROM** Customers -- **WHERE** City='Berlin';

  - 다음 예제에서는 한 줄 주석을 사용하여 `SELECT` 문을 무시한다.

    > -- SELECT * FROM Customers;
    >
    > **SELECT** * **FROM** Products;

- **여러 줄 주석** ( Multi-line Comments )

  - 여러 줄 주석은  `/*`로 시작하고  `*/`로 끝나고, `/*`와  ` */` 사이의 모든 텍스트는 무시된다.

  - 다음 예에서는 설명으로 여러 줄 주석을 사용한다.

    > /* Select all the columns
    > of all the records
    > in the Customers table: */
    > **SELECT** * **FROM** Customers;

  - 다음 예에서는 여러 줄 주석을 사용하여 많은 구문을 무시한다.

    > /*** SELECT** * **FROM** Customers;
    > **SELECT** * **FROM** Products;
    > **SELECT** * **FROM** Orders;
    > **SELECT** * **FROM** Categories; */
    > **SELECT** * **FROM** Suppliers;

  - 명령문의 일부만 무시하려면 `/* */` 주석을 사용할 수 있다.

    > **SELECT** CustomerName, /* City, */ Country **FROM** Customers;

  - 다음 예에서는 주석을 사용하여 구문의 일부를 무시한다.

    > **SELECT** * **FROM** Customers **WHERE** (CustomerName **LIKE** 'L%'
    > **OR** CustomerName **LIKE** 'R%' /* OR CustomerName **LIKE** 'S%'
    > **OR** CustomerName **LIKE** 'T%' */ **OR** CustomerName **LIKE** 'W%')
    > **AND** Country='USA'
    > **ORDER BY** CustomerName;

- **SQL 연산자 모음**

  - **SQL 산술 연산자**

    | 연산자 | 설명            |
    | ------ | --------------- |
    | +      | 더한다          |
    | -      | 뺀다            |
    | *      | 곱한다          |
    | /      | 나눈다          |
    | %      | 나머지를 구한다 |

  - **SQL 비트 연산자**

    | 연산자 | 설명                   |
    | ------ | ---------------------- |
    | &      | 비트 AND               |
    | \|     | 비트 OR                |
    | ^      | 비트 XOR ( 배타적 OR ) |

  - **SQL 비교 연산자**

    | 연산자 | 설명              |
    | ------ | ----------------- |
    | =      | 같다              |
    | >      | ~보다 크다        |
    | <      | ~보다 작다        |
    | >=     | ~보다 크거나 같다 |
    | <=     | ~보다 작거나 같다 |
    | <>     | 같지 않다         |

  - **SQL 복합 연산자**

    | 연산자 | 설명                                                         |
    | ------ | ------------------------------------------------------------ |
    | +=     | ( 더하기 대입 )  원래 값에 특정 값을 더하고 원래 값을 연산 결과로 설정한다. |
    | -=     | ( 빼기 대입 )   원래 값에 특정 값을 빼고 원래 값을 연산 결과로 설정한다. |
    | *=     | ( 곱하기 대입 )  특정 값으로 곱하고 원래 값을 연산 결과로 설정한다. |
    | /=     | ( 나누기 대입 )  특정 값으로 나누고 원래 값을 연산 결과로 설정한다. |
    | %=     | ( 모듈러스 대입 )  특정 값으로 곱하고 원래 값을 연산 결과로 설정한다. |
    | &=     | ( 비트 AND 대입 )  비트 AND를 수행하고 원래 값을 연산 결과로 설정한다. |
    | \|*=   | ( 비트 OR 대입 )  비트 OR를 수행하고 원래 값을 연산 결과로 설정한다. |
    | ^-=    | ( 배타적 비트 OR 대입 )  배타적 비트 OR를 수행하고 원래 값을 연산 결과로 설정한다. |

  - **SQL 논리 연산자**

    | 연산자  | 설명                                                |
    | ------- | --------------------------------------------------- |
    | ALL     | 모든 서브 쿼리 값이 조건을 충족하는 경우 TRUE       |
    | ANY     | 서브 쿼리 값 중 하나라도 조건을 충족하는 경우 TRUE  |
    | SOME    | 서브 쿼리 값 중 하나라도 조건을 충족하는 경우 TRUE  |
    | AND     | AND로 구분된 모든 조건이 TRUE이면 TRUE              |
    | OR      | OR로 구분된 조건 중 하나라도 TRUE이면 TRUE          |
    | EXISTS  | 서브 쿼리가 하나 이상의 레코드를 반환하는 경우 TRUE |
    | IN      | 피연산자가 표현식 목록 중 하나와 같으면 TRUE        |
    | LIKE    | 피연산자가 패턴과 일치하면 TRUE                     |
    | BETWEEN | 피연산자가 비교 범위 내에 있으면 TRUE               |
    | NOT     | 조건이 TRUE가 아닌 경우 레코드를 표시               |

- **SQL Create DB**

  - `CREATE DATABASE` 문은 새 SQL 데이터베이스를 만드는 데 사용된다.

    > **CREATE DATABASE** databasename;

  - 다음 SQL 문은 `"testDB"`라는 데이터베이스를 생성한다.

    > **CREATE DATABASE** testDB;

  - ( 팁! ) 데이터베이스를 생성하기 전에 관리자 권한이 있는지 확인하자. 데이터베이스가 생성되면 다음 SQL 명령을 통해 데이터베이스 목록에서 생성된 DB를 확인할 수 있다.

    > **SHOW DATABASES**;

- **SQL DROP DATABASE 문**

  - `DROP DATABASE` 문은 기존 SQL 데이터베이스를 삭제하는 데 사용된다.

    > **DROP DATABASE** databasename;

  - ( 참고 )  데이터베이스를 삭제하기 전에 주의하자. 데이터베이스를 삭제하면 데이터베이스에 저장된 모든 정보가 손실된다!

  - 다음 SQL 문은 기존 데이터베이스 `"testDB"`를 삭제한다.

    > **DROP DATABASE** testDB;

  - ( 팁 )  데이터베이스를 삭제하기 전에 **관리자 권한**이 있는지 확인하자. 데이터베이스가 삭제되면 다음 SQL 명령을 사용하여 데이터베이스 목록에서 확인할 수 있다. 

    > **SHOW DATABASES**;

- **SQL BACKUP DATABASE 문**

  - `BACKUP DATABASE` 문은 SQL Server에서 기존 SQL 데이터베이스의 전체 백업을 만드는 데 사용된다.

    > **BACKUP DATABASE** databasename
    >
    > **TO DIST** = 'filepath';

  - **차등 백업** 은 마지막 전체 데이터베이스 백업 이후 **변경된 데이터베이스 부분만 백업** 한다.

    > **BACKUP DATABASE** databasename
    > **TO DISK** = '*filepath*'
    > **WITH DIFFERENTIAL**;

  - 다음 SQL 문은 기존 데이터베이스 `"testDB"`의 전체 백업을 D 디스크에 생성한다.

    > **BACKUP DATABASE** testDB
    >
    > **TO DISK** = 'D:\backups\testDB.bak';

  - ( 팁 )  항상 실제 데이터베이스와 다른 드라이브에 데이터베이스를 백업하자. 그러면 디스크 충돌이 발생해도 데이터베이스와 함께 백업 파일이 손실되지 않는다.

  - 다음 SQL 문은 `"testDB"` 데이터베이스의 차등 백업을 생성한다.

    > **BACKUP DATABASE** testDB
    >
    > **TO DISK** = 'D:\backups\testDB.bak'
    >
    > **WITH DIFFERENTIAL**;

  - ( 팁 )  차등 백업은 백업 시간을 줄인다 (변경 사항만 백업되기 때문에).

- **SQL CREATE TABLE 문**

  - `CREATE TABLE` 문은 데이터베이스에 새 테이블을 만드는 데 사용된다.

    > **CREATE TABLE** table_name (
    >
    > 		column1 datatype,
    > 	
    > 		column2 datatype,
    > 	
    > 		column3 datatype,
    > 	
    > 		...
    >
    > );

    - 컬럼 매개변수는 테이블의 컬럼 이름을 지정한다.
    - `datatype` 매개변수는 컬럼이 보유할 수 있는 데이터 유형(예: varchar, 정수, 날짜 등)을 지정한다.
    - 팁: 사용 가능한 데이터 유형에 대한 개요를 보려면 전체 [데이터 유형 참조](https://www.w3schools.com/sql/sql_datatypes.asp)하자.

  - 다음 예에서는 `PersonID`, `LastName`, `FirstName`, `Address` 및 `City`의 5개 열이 포함된 `"Persons"`라는 테이블을 만든다.

    > **CREATE TABLE** Persons (
    >
    > 		PersonID int,
    > 	
    > 		LastName varchar(255),
    > 	
    > 		FirstName varchar(255),
    > 	
    > 		Address varchar(255),
    > 	
    > 		City varchar(255)
    >
    > );

  - 이제 비어 있는 `"Persons"` 테이블이 다음과 같이 표시된다.

    ![image](https://user-images.githubusercontent.com/49539592/128461281-c8a449b2-1f1a-4d26-bf4c-b8b1055c840c.png)

  - 팁: 이제 `INSERT INTO` 문을 사용하여 빈 `"Persons"` 테이블을 데이터로 채울 수 있다.

  - `CREATE TABLE`을 사용하여 기존 테이블의 복사본을 만들 수도 있다.

    - 새 테이블은 동일한 컬럼 정의를 가져온다. 모든 컬럼을 가져오거나 또는 특정 컬럼을 가져올 수 있다.

    - 기존 테이블을 사용하여 새 테이블을 만드는 경우 새 테이블은 이전 테이블의 기존 값으로 채워진다.

      > **CREATE TABLE** new_table_name **AS**
      >
      > 		**SELECT** column1, column2, ...
      > 	
      > 		**FROM** existing_table_name
      > 	
      > 		**WHERE** .... ;

    - 다음 SQL은 `"TestTable"`(`"Customers"` 테이블의 복사본)라는 새 테이블을 만든다.

      > **CREATE TABLE** TestTable **AS**
      >
      > 		**SELECT** customername, contactname
      > 	
      > 		**FROM** Customers;

- **SQL DROP TABLE 문**

  - `DROP TABLE` 문은 데이터베이스의 기존 테이블을 삭제하는 데 사용된다.

    > **DROP TABLE** table_name;

  - 참고: 테이블을 떨어뜨리기 전에 주의하자. 테이블을 삭제하면 테이블에 저장된 모든 정보가 손실된다!

  - 다음 SQL 문은 기존 테이블 "Shippers"를 삭제한다.

    > **DROP TABLE** Shippers;

- **SQL TRUNCATE TABLE 문**

  - `TRUNCATE TABLE` 문은 테이블 내부의 데이터를 삭제하는 데 사용되지만 테이블 자체는 삭제하지 않는다.

    > **TRUNCATE TABLE** table_name;

- **SQL ALTER TABLE 문**

  - `ALTER TABLE` 문은 **기존 테이블의 열을 추가, 삭제 또는 수정** 하는 데 사용된다.
  - `ALTER TABLE` 문은 또한 기존 테이블에 **다양한 제약 조건을 추가 및 삭제** 하는 데 사용된다.

- **ALTER TABLE - 컬럼 추가**

  > **ALTER TABLE** table_name
  >
  > **ADD** column_name datatype;

  - 다음 SQL은 `"Customers"` 테이블에 `"Email"` 열을 추가한다.

  - 새로 만들어진 컬럼의 값은 `null`로 채워진다.

    > **ALTER TABLE** table_name
    >
    > **ADD** Email varchar (255);

- **ALTER TABLE - 컬럼 삭제**

  > **ALTER TABLE** table_name
  >
  > **DROP COLUMN** column_name;

  - 다음 SQL은 `"Customers"` 테이블에서 `"Email"` 열을 삭제한다.

    > **ALTER TABLE** Customers
    >
    > **DROP COLUMN** Email;

- **ALTER TABLE - 컬럼 변경/수정**

  - **SQL Server  /  MS Access**

    > **ALTER TABLE** table_name
    >
    > **ALTER COLUMN** column_name datatype;

  - **My SQL  /  Oracle  (10G 이전 버전)**

    > **ALTER TABLE** table_name
    >
    > **MODIFY COLUMN** column_name datatype;

  - **Oracle  (10G 버전 이후)**

    > **ALTER TABLE** table_name
    >
    > **MODIFY** column_name datatype;

- **SQL 제약 조건(Constraints)**

  - **SQL 제약 조건** 은 테이블의 **데이터에 대한 규칙** 을 지정하는 데 사용된다.
  - 제약 조건은 테이블에 들어갈 수 있는 **데이터 유형을 제한** 하는 데 사용된다. 이렇게 하면 테이블에 있는 **데이터의 정확성과 신뢰성** 이 보장된다. 제약 조건과 데이터 작업 사이에 위반이 있으면 작업이 중단된다.
  - 제약 조건은 **컬럼 수준 또는 테이블 수준** 일 수 있다.

- **SQL 제약 조건 생성**

  - 제약 조건은 `CREATE TABLE` 문으로 테이블을 생성할 때 또는 `ALTER TABLE` 문으로 테이블을 생성한 후에 지정할 수 있다.

    > **CREATE TABLE** table_name (
    >
    > 		column1 datatype constraint,
    > 	
    > 		column2 datatype constraint,
    > 	
    > 		column3 datatype constraint,
    > 	
    > 		....
    >
    > );

  - 다음 제약 조건은 SQL에서 일반적으로 사용된다.

    - **NOT NULL** - 열이 NULL 값을 가질 수 없도록 한다.
    - **UNIQUE** - 열의 모든 값이 서로 다른지 확인한다.
    - **PRIMARY KEY** - NOT NULL과 UNIQUE의 조합. 테이블의 각 행을 고유하게 식별한다.
    - **FOREIGN KEY** - 테이블 간의 링크를 파괴하는 작업을 방지한다.
    - **CHECK** - 열의 값이 특정 조건을 충족하는지 확인한다.
    - **DEFAULT** - 값이 지정되지 않은 경우 열의 기본값을 설정한다.
    - **CREATE INDEX** - 데이터베이스에서 매우 빠르게 데이터를 생성하고 검색하는 데 사용된다.

- **SQL NOT NULL 제약 조건**

  - 기본적으로 열은 NULL 값을 보유할 수 있다. NOT NULL 제약 조건은 열이 NULL 값을 허용하지 않도록 한다.

  - 이렇게 하면 필드에 항상 값이 포함된다. 즉, 이 필드에 값을 추가하지 않고는 새 레코드를 삽입하거나 레코드를 업데이트할 수 없다.

  - **CREATE TABLE에서  NOT NULL 제약 조건 넣기**

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar (255) **NOT NULL**,	FirstName varchar (255) **NOT NULL**,	Age int
    >
    > );

  - **ALTER TABLE에서  NOT NULL 제약 조건 넣기**

    > **ALTER TABLE** Persons
    >
    > **MODIFY** Age int **NOT NULL**;

- **SQL UNIQUE 제약 조건**

  - UNIQUE 제약 조건은 **열의 모든 값이 서로 다른지 확인** 한다.

  - UNIQUE 및 PRIMARY KEY 제약 조건 모두 **컬럼 또는 컬럼 집합의 고유성** 을 보장한다.

  - **PRIMARY KEY 제약 조건에는 자동으로 UNIQUE 제약 조건이 있다.**

  - 테이블당 여러 UNIQUE 제약 조건을 가질 수 있지만, **PRIMARY KEY는 테이블당 오직 하나** 의 제약 조건만 가질 수 있다.

  - **CREATE TABLE에  UNIQUE 제약 조건 넣기**

    - 다음 SQL은 "Persons" 테이블이 생성될 때 "ID" 열에 대한 UNIQUE 제약 조건을 생성한다.

    - **SQL Server  /  Oracle  /  MS Access**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL UNIQUE**,
      > 	
      > 		LastName varchar(255) **NOT NULL**,
      > 	
      > 		FirstName varchar(255),
      > 	
      > 		Age int
      >
      > );

    - **MySQL**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL**,
      > 	
      > 		LastName varchar(255) **NOT NULL**,
      > 	
      > 		FirstName varchar(255),
      > 	
      > 		Age int,
      > 	
      > 		**UNIQUE** (ID)
      >
      > );

    - UNIQUE 제약 조건의 이름을 지정하고 여러 열에 대한 UNIQUE 제약 조건을 정의하려면 다음 SQL 구문을 사용한다.

    - **MySQL / SQL Server / Oracle / MS Access**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**CONSTRAINT** UC_Person **UNIQUE**  (ID, LastName)
      >
      > );

  - **ALTER TABLE에  UNIQUE 제약 조건 넣기**

    - **MySQL / SQL Server / Oracle / MS Access**

      > **ALTER TABLE** Persons
      >
      > **ADD UNIQUE** (ID);

  - UNIQUE 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 UNIQUE 제약 조건** 을 정의하려면 다음 SQL 구문을 사용한다.

    - **MySQL / SQL Server / Oracle / MS Access**

      > **ALTER TABLE** Persons
      >
      > **ADD CONSTRAINT** UC_Person **UNIQUE** (ID, LastName);

  - **UNIQUE 제약 조건 삭제**

    - **MySQL**

      > **ALTER TABLE** Persons
      >
      > **DROP INDEX** UC_Person;

    - **SQL Server  /  Oracle  /  MS Access**

      > **ALTER TABLE** Persons
      >
      > **DROP CONSTRAINT** UC_Person;

- **SQL PRIMARY KEY 제약 조건**

  - PRIMARY KEY 제약 조건은 테이블의 각 레코드를 고유하게 식별한다.
  - **기본 키는 UNIQUE 값을 포함해야 하며 NULL 값을 포함할 수 없다.**
  - 테이블에는 하나의 기본 키만 있을 수 있다. 테이블에서 이 **기본 키는 단일 또는 다중 열(필드)로 구성될 수 있다.**

- **CREATE TABLE의 SQL 기본 키**

  - 다음 SQL은 "Persons" 테이블이 생성될 때 "ID" 열에 PRIMARY KEY를 생성한다.

    - **MySQL**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**PRIMARY KEY** (ID)
      >
      > );

    - **SQL Server  /  Oracle  /  MS Access**

      > **CREATE TABLE** Persons (
      >
      > 		ID int **NOT NULL PRIMARY KEY**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int
      >
      > );

  - PRIMARY KEY 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 PRIMARY KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용하자.

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**CONSTRAINT** PK_Person **PRIMARY KEY** (ID, LastName)
    >
    > );

  - 참고: 위의 예에는 하나의 PRIMARY KEY(PK_Person)만 있다. 그러나 기본 키의 VALUE는 2개의 COLUMNS(ID + LastName)로 구성되어 있다.

- **ALTER TABLE의 SQL 기본 키**

  > **ALTER TABLE** Persons
  >
  > **ADD PRIMARY KEY**  (ID);

  - PRIMARY KEY 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 PRIMARY KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용하자.

    > **ALTER TABLE** Persons
    >
    > **ADD CONSTRAINT** PK_Person **PRIMARY KEY** (ID, LastName);

  - 참고: ALTER TABLE을 사용하여 기본 키를 추가하는 경우 기본 키 열은 (테이블이 처음 생성되었을 때) NULL 값을 포함하지 않도록 선언해야 한다.

- **PRIMARY KEY 제약 조건 삭제**

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **DROP PRIMARY KEY**;

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Persons
    >
    > **DROM CONSTRAINT** PK_Person;

- **SQL FOREIGN KEY 제약 조건**

  - FOREIGN KEY 제약 조건은 **테이블 간의 링크를 파괴하는 작업을 방지** 하는 데 사용된다.

  - FOREIGN KEY는 **다른 테이블의 PRIMARY KEY를 참조하는 한 테이블의 필드(또는 필드 모음)** 이다.

  - 외래 키가 있는 테이블을 **자식 테이블** 이라고 하고, 기본 키가 있는 테이블을 **참조 또는 부모 테이블** 이라고 한다.

    ![image](https://user-images.githubusercontent.com/87057868/128487519-619f6187-30ef-4108-8e68-c318c01fa6d1.png)

    ![image](https://user-images.githubusercontent.com/87057868/128487606-bdfc1b7a-c49d-4d22-a06c-f1464f1677ee.png)

  - "Orders" 테이블의 "PersonID" 열은 "Persons" 테이블의 "PersonID" 열을 가리킨다.

  - "Persons" 테이블의 "PersonID" 열은 "Persons" 테이블의 PRIMARY KEY이다.

  - "Orders" 테이블의 "PersonID" 열은 "Orders" 테이블의 FOREIGN KEY이다.

  - FOREIGN KEY 제약 조건은 부모 테이블에 포함된 값 중 하나여야 하므로 외래 키 열에 잘못된 데이터가 삽입되는 것을 방지한다.

- **CREATE TABLE의 FOREIGN KEY**

  - 다음 SQL은 "Orders" 테이블이 생성될 때 "PersonID" 열에 FOREIGN KEY를 생성한다.

  - **MySQL**

    > **CREATE TABLE** Orders (
    >
    > 		OrderID int **NOT NULL**,	OrderNumber int **NOT NULL**,	PersonID int,	**PRIMARY KEY** (OrderID),	**FOREIGN KEY** (PersonID) **REFERENCES** (PersonID)
    >
    > );

  - **SQL Server  /  Oracle  /  MS Access**

    > **CREATE TABLE** Orders (
    >
    > 		OrderID int **NOT NULL PRIMARY KEY**,	OrderNumber int **NOT NULL**,	PersonID int **FOREIGN KEY REFERENCES** Person(PersonID)
    >
    > );

  - FOREIGN KEY 제약 조건의 **이름 지정을 허용** 하고 **여러 열에 대한 FOREIGN KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용하자.

    > **CREATE TABLE** Orders (
    >
    > 		OrderID int **NOT NULL**,	OrderNumber int **NOT NULL**,	PersonID int,	**PRIMARY KEY** (OrderID),	**CONSTRAINT** FK_PersonOrder **FOREIGN KEY** (PersonID)	**REFERENCES** Persons(PersonID)
    >
    > );

- **CREATE TABLE의 FOREIGN KEY**

  - "Orders" 테이블이 이미 생성된 경우 "PersonID" 열에 대한 FOREIGN KEY 제약 조건을 생성하려면 다음 SQL을 사용한다.

    > **ALTER TABLE** Orders
    >
    > **ADD FOREIGN KEY** (PersonID) **REFERENCES** Persons (PersonID);

  - FOREIGN KEY 제약 조건의 **이름 지정** 을 허용하고 **여러 열에 대한 FOREIGN KEY 제약 조건을 정의** 하려면 다음 SQL 구문을 사용한다.

    > **ALTER TABLE** Orders
    >
    > **ADD CONSTRAINT** FK_PersonOrder
    >
    > **FOREIGN KEY** (PersonID) **REFERENCES** Persons(PersonID);

- **FOREIGN KEY 제약 조건 삭제**

  - **MySQL**

    > **ALTER TABLE** Orders
    >
    > **DROP FOREIGN KEY** FK_PersonOrder;

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Orders
    >
    > **DROP CONSTRAINT** FK_PersonOrder;

- **SQL CHECK 제약 조건**

  - CHECK 제약 조건은 컬럼에 넣을 수 있는 **값의 범위를 제한** 하기 위해 사용한다.
  - 열에 CHECK 제약 조건을 정의하면 이 열에 대해 특정 값만 허용된다.
  - 테이블에 CHECK 제약 조건을 정의하면 행의 다른 컬럼의 값을 기반으로 특정 컬럼의 값을 제한할 수 있다.

- **CREATE TABLE에  CHECK 제약 조건 넣기**

  - 다음 SQL은 "Person" 테이블이 생성될 때 "Age" 열에 대한 CHECK 제약 조건을 생성한다. CHECK 제약 조건은 사람의 연령이 18세 이상이어야 함을 보장한다.

  - **MySQL**

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	**CHECK** (Age>=18)
    >
    > );

  - **SQL Server  /  Oracle  /  MS Access**

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int **CHECK** (Age >= 18)
    >
    > );

  - CHECK 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 CHECK 제약 조건을 정의** 하려면 다음 SQL 구문을 사용한다.

    > **CREATE TABLE** Persons (
    >
    > 		ID int **NOT NULL**,
    > 	
    > 		LastName varchar(255) **NOT NULL**,
    > 	
    > 		FirstName varchar(255),
    > 	
    > 		Age int,
    > 	
    > 		City varchar(255),
    > 	
    > 		**CONSTRAINT** CHK_Person **CHECK** (Age>=18 **AND** City='Sandnes')
    >
    > );

- **ALTER TABLE에  CHECK 제약 조건 넣기**

  > **ALTER TABLE** Persons
  >
  > **ADD CHECK** (Age >= 18);

  - CHECK 제약 조건의 **이름을 지정** 하고 **여러 열에 대한 CHECK 제약 조건을 정의** 하려면 다음 SQL 구문을 사용한다.

    > **ALTER TABLE** Persons
    >
    > **ADD CONSTRAINT** CHK_PersonAge **CHECK** (Age>=18 **AND** City='Sandnes');

- **CHECK 제약 조건 삭제**

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Persons
    >
    > **DROP CONSTRAINT** CHK_PersonAge;

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **DROP CHECK** CHK_PersonAge;

- **SQL DEFAULT 제약 조건**

  - DEFAULT 제약 조건은 컬럼의 기본값을 설정하는 데 사용된다.
  - 다른 값을 지정하지 않으면 기본값이 모든 새 레코드에 추가된다.

- **CREATE TABLE에 DEFAULT 넣기**

  - 다음 SQL은 "Persons" 테이블이 생성될 때 "City" 열에 대해 DEFAULT 값을 설정한다.

    > **CREATE TABLE** Persons (
    >
    > 		**ID** int **NOT NULL**,	LastName varchar(255) **NOT NULL**,	FirstName varchar(255),	Age int,	City varchar(255) **DEFAULT** 'Sandnes'
    >
    > );

  - DEFAULT 제약 조건은 GETDATE()와 같은 함수를 사용하여 시스템 값을 삽입하는 데에도 사용할 수 있다.

    > **CREATE TABLE** Orders (
    >
    > 		ID int **NOT NULL**,	OrderNumber int **NOT NULL**,	OrderDate date **DEFAULT** GETDATE()
    >
    > );

- **ALTER TABLE에 DEFAULT 넣기**

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **ALTER** City **SET DEFAULT** 'Sandnes';

  - **SQL Server**

    > **ALTER TABLE** Persons
    >
    > **ADD CONSTRAINT** df_City
    >
    > **DEFAULT** 'Sandnes' FOR City;

  - **MS Access**

    > **ALTER TABLE** Persons
    >
    > **ALTER COLUMN** City **SET DEFAULT** 'Sandnes';

  - **Oracle**

    > **ALTER TABLE** Persons
    >
    > **MODIFY** City **DEFAULT** 'Sandnes';

- **DEFAULT 제약 조건 삭제**

  - **MySQL**

    > **ALTER TABLE** Persons
    >
    > **ALTER** City **DROP DEFAULT**;

  - **SQL Server  /  Oracle  /  MS Access**

    > **ALTER TABLE** Persons
    >
    > **ALTER COLUMN** City **DROP DEFAULT**;


# References

- [SQL Tutorial - w3schools.com](https://www.w3schools.com/sql/default.asp) 

