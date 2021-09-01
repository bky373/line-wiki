# SQL

## SQL Aliases ( 별칭 )

  - SQL 별칭은 **테이블** 이나 **테이블의 열** 에  **임시 이름** 을 지정하는 데 사용된다.

  - 별칭은 `AS` 키워드를 통해 생성되며, **해당 쿼리 기간 동안에만** 존재한다.

  - **컬럼의 별칭 사용**

    > **SELECT** column_name **AS** alias_name
    >
    > **FROM** table_name;
    >
    > 
    >
    > (실사용 예시)
    >
    > **SELECT** CustomerID **AS** ID, CustomerName **AS** Customer
    >
    > **FROM** Customers;

    ![image](https://user-images.githubusercontent.com/49539592/127871370-711e0485-0dbc-4f5d-8ced-15e344a1f308.png)

    

  - 별칭 이름에 **공백** 이 포함된 경우,  **큰 따옴표** 나 **대괄호** 를 사용해야 한다.

    > **SELECT** CustomerID **AS** ID, ContactName **AS** [Contact Person]
    >
    > **FROM** Customers;

    ![image](https://user-images.githubusercontent.com/49539592/127871868-f31caefa-f2e0-44b8-b6da-3baf99d7ee94.png)

  - 다음 SQL 문은 4개의 열(`Address`, `PostalCode`, `City` 및 `Country`)을 결합하여 "`Address`"라는 별칭을 만든다. 

    > **SELECT** CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country **AS** Address
    > **FROM** Customers;
    >
    > 
    >
    > -- 아래 SQL문은 `MySQL` 기준
    >
    > **SELECT** CustomerName, **CONCAT**(Address, ', ', PostalCode, ', ', City, ', ', Country) **AS** Address
    > **FROM** Customers;

  - **테이블의 별칭 사용**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name **AS** alias_name;
    >
    > (실사용 예시)
    >
    > **SELECT** o.OrderID, o.OrderDate, c.CustomerName
    > **FROM** Customers **AS** c, Orders **AS** o
    > **WHERE** c.CustomerName='Blondel père et fils' **AND** c.CustomerID=o.CustomerID;

    - 위의 SQL 문은 `CustomerName`이  `Blondel père et fils`이고,  `CustomerID`가  7번인 고객의 모든 주문을 선택한다. 이때 `Customers` 와  `Orders` 테이블이 사용되는데,  각각  `c` 와  `o`라는 별칭을 갖는다.
    - 여기서 `c` 와  `o`라는 별칭을 사용함으로써  SQL 문을 더 짧게 만들어 사용할 수 있다.

# References

- [SQL Aliases - w3schools.com](https://www.w3schools.com/sql/sql_alias.asp) 
