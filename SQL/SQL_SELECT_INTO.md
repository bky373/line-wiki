# SQL

## SQL SELECT INTO 문

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



# References

- [SQL Select Into - w3schools.com](https://www.w3schools.com/sql/sql_select_into.asp) 
