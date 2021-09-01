# SQL

## IN 연산자

  - `IN` 연산자를 사용하면  `WHERE` 절에 **여러 개의 값** 을 지정할 수 있다.

  - `IN` 연산자는 여러 개의  `OR` 조건의 약어이다.

    > **SELECT**  column_name(s)
    >
    > **FROM**  table_name
    >
    > **WHERE**  column_name **IN** (value1, value2, ...);
    >
    > 
    >
    > (실사용 예시)
    >
    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **WHERE**  Country  **IN** ('Germany', 'France', 'UK');
    >
    > 
    >
    > ( **NOT 연산자** 와 함께 )
    >
    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **WHERE**  Country  **NOT IN** ('Germany', 'France', 'UK');

    또는

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** column_name **IN** (**SELECT** STATEMENT);
    >
    > 
    >
    > (실사용 예시)
    >
    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **WHERE**  Country  **IN** (**SELECT** Country **FROM** Suppliers);
    
    
# References

- [SQL In - w3schools.com](https://www.w3schools.com/sql/sql_in.asp) 
