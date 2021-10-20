# SQL

## SQL ALTER TABLE 문

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


# References

- [SQL Alter Table - w3schools.com](https://www.w3schools.com/sql/sql_alter.asp) 
