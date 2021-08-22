# SQL

## INSERT INTO 문

  - `INSERT INTO` 문은 테이블에 새 레코드를 삽입하는 데 사용된다. 아래와 같이 두 가지 방법으로 쿼리를 작성할 수 있다.

    1. 삽입할 컬럼의 이름과 값을 모두 지정한다.

       > **INSERT  INTO**  table_name  (column1,  column2,  column3,  ... )
       >
       > **VALUES**  (value1,  value2,  value3,  ... );

    2. 모든 컬럼에 값을 넣는다면 굳이 컬럼의 이름을 나열할 필요 없이 값만 넣어주면 된다. 단, 이때 값의 순서는 컬럼의 순서와 일치해야 한다.

       > **INSERT  INTO**  table_name
       >
       > **VALUES**  (value1,  value2,  value3,  ...);

# References

- [SQL Insert Into - w3schools.com](https://www.w3schools.com/sql/sql_insert.asp) 

