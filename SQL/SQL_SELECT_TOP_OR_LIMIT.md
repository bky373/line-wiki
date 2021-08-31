# SQL

- **SELECT TOP 또는 LIMIT 절**

  - `SELECT TOP` 절은 반환할 레코드 수를 지정하는 데 사용된다.

  - `SELECT TOP` 절은 수천 개의 레코드가 있는 큰 테이블에서 유용하다.  목적 없이 많은 수의 레코드를 반환하면 성능에 영향을 주기 때문이다.

  - 다만 모든 데이터베이스 시스템이 `SELECT TOP`  절을 지원하지는 않는다. `MySQL` 은 제한된 수의 레코드를 선택하기 위해 `LIMIT` 을 사용하지만,  `Oracle` 은 `FETCH FIRST n ROWS ONLY`  또는  `ROWNUM` 을 사용한다.

  - **MySQL 문법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > ( **WHERE** condition )
    >
    > **LIMIT** number;
    >
    > 
    >
    >
    > (실사용 예시)
    >
    > **SELECT** *
    >
    > **FROM** Customers
    >
    > **LIMIT** 3;

  - **SQL Server / MS Access 문법**

    > **SELETC TOP** number|**PERCENT** column_name(s)
    >
    > **FROM**  table_name
    >
    > **WHERE**  condition;

  - **Oracle 12 문법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **ORDER BY** column_name(s)
    >
    > **FETCH** **FIRST**  number|**PERCENT**  **ROWS ONLY**;

  - **이전 Oracle 문법**

    > **SELECT** column_name(s)
    >
    > **FROM** table_name
    >
    > **WHERE** ROWNUM <= number;

  - 숫자 뒤에 `PERCENT` 키워드를 작성하는 경우, 해당 숫자의 퍼센트 만큼 레코드를 반환한다.

    > -- 총 레코드 수(91개)의 절반인 46개의 레코드를 반환 
    >
    > **SELECT TOP** 50 **PERCENT** * 
    >
    > **FROM** Customers;
    

# References

- [SQL Select Top - w3schools.com](https://www.w3schools.com/sql/sql_top.asp) 

