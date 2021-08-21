# SQL

## ORDER BY 연산자

  - `ORDER BY`  키워드는 오름차순 또는 내림차순으로 결과물을 정렬하기 위해 사용한다.

  - 오름차순 정렬을 하려면  `ASC`,  내림차순 정렬을 하려면 `DESC`  키워드를 사용한다.  ( **기본 ( `DEFAULT` ) 정렬은 오름차순** 이다. ) 

    > **SELECT**  column1,  column2,  ...
    >
    > **FROM**  table_name
    >
    > **ORDER  BY**  column1,  column2,   ...   ASC|DESC;

  - 여러 개의 컬럼 정렬 사용 예

    > **SELECT**  *
    >
    > **FROM**  Customers
    >
    > **ORDER BY**  Country  ASC,  CustomerName; 

# References

- [SQL AND, OR and NOT - w3schools.com](https://www.w3schools.com/sql/sql_orderby.asp) 
