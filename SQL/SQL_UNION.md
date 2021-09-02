# SQL

## SQL UNION 연산자

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


# References

- [SQL Union - w3schools.com](https://www.w3schools.com/sql/sql_union.asp) 
