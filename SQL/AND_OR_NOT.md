# SQL

## AND, OR , NOT  연산자

  - `WHERE` 절은  `AND`,  `OR`,  `NOT`  연산자와 결합하여 사용할 수 있다.

  - `AND`,  `OR`   연산자는 둘 이상의 조건을 기반으로 레코드를 필터링하는 데 사용된다.

    - `AND`  연산자는  `AND`  양 옆의 조건이 **모두**  `TRUE` 인 레코드를 표시한다.

      > **SELECT**  column1,  column2,  ...
      >
      > **FROM**  table_name
      >
      > **WHERE**  condition1  AND  condition2  AND  condition3  ... ;

    - `OR`  연산자는   `OR`  양 옆의 조건 중 **하나라도**  `TRUE`    인 레코드를 표시한다.

      > **SELECT**  column1,  column2,  ...
      >
      > **FROM**  table_name
      >
      > **WHERE**  condition1  OR   condition2   OR   condition3   ... ;

    - `NOT`  연산자는  `NOT`  다음의 **조건이 일치하지 않는** 레코드를 표시한다. 

      > **SELECT**  column1,  column2,  ...
      >
      > **FROM**  table_name
      >
      > **WHERE  NOT**  condition;

  - `AND`,  `OR`,  `NOT`  연산자를 결합해서 사용할 수도 있다.

    - 다음 SQL 문은 고객 테이블에서 국가가  `독일` 이고,  도시가  `베를린`  또는  `뮌헨` 인  모든 레코드를 얻는다.

      > **SELECT**  *
      >
      > **FROM**  Customers
      >
      > **WHERE**  Country='Germany'   AND  (City='Berlin'  OR  City='München');  

# References

- [SQL AND, OR and NOT - w3schools.com](https://www.w3schools.com/sql/sql_and_or.asp)
