# SQL

## **UPDATE 문**

  - `UPDATE` 문은 테이블의 기존 레코드를 수정하기 위해 사용한다.

    - ( 주의! )  `UPDATE` 문을 사용할 때 업데이트해야 하는 레코드를 **`WHERE` 절을 통해 지정** 해야 한다.  만약  `WHERE` 절을 생략하면 테이블의 모든 레코드가 업데이트된다!

    > **UPDATE**  table_name
    >
    > **SET**  column1=value1,  column2=value2,  ...
    >
    > **WHERE**  condition;
    >
    > (실제 사용 예시)
    >
    > **UPDATE**  Customers
    >
    > **SET**  ContactName='Alfred Schmidt',  City='Frankfurt'
    >
    > **WHERE**  CustomerID = 1;

  - `WHERE` 절은 업데이트할 레코드 수를 결정한다. 아래 SQL 문은 국가가 'Mexico' 인 모든 레코드의 `ContactName` 을  'Juan' 으로 업데이트한다.

    > **UPDATE** Customers
    > **SET** ContactName='Juan'
    > **WHERE** Country='Mexico';



# References

- [SQL Update - w3schools.com](https://www.w3schools.com/sql/sql_update.asp) 
