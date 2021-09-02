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

# References

- [SQL Union - w3schools.com](https://www.w3schools.com/sql/sql_union.asp) 
