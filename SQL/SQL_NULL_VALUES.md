# SQL

## NULL 값

  - `NULL` 값이 있는 필드는 말그대로 **값이 없는 필드** 를 나타낸다.

    > `NULL` 값은  ( 0  또는 공백 ) 과는 다르다.
    >
    > `NULL` 값이 있는 필드는 **생성 중에 비어 있는** 필드를 의미한다.

  - 테이블의 필드가  `Optional` 인 경우 이 필드에 값을 추가하지 않고 새 레코드를 삽입하거나 레코드를 업데이트할 수 있다. 이 경우 필드의 값이 `NULL` 로 저장된다.

  - `NULL` 값 인지의 여부는  오직  `IS NULL`  및  `IS NOT NULL`  연산자를 통해 알 수 있다.

    - ( Tip )  항상  `IS NULL` 을 사용하여  `NULL` 값을 찾도록 하자.

    > **SELECT**  column_names
    >
    > **FROM**  table_name
    >
    > **WHERE**  column_name  IS NULL|IS NOT NULL;
    >
    > (실제 사용 예시)
    >
    > **SELECT**  CustomerName,  ContactName,  Address
    >
    > **FROM**  Customers
    >
    > **WHERE**  Address  IS NULL;

    > 이때  `=`,  `<`   또는  `<>` 와 같은 비교 연산자를 통해 `NULL` 값인지 알아낼 수 없다.

# References

- [SQL Null Values - w3schools.com](https://www.w3schools.com/sql/sql_null_values.asp) 
