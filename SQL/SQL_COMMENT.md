# SQL

## SQL COMMENTS

- **주석** 은 SQL 문의 섹션을 설명하거나 SQL 문의 실행을 방지하는 데 사용된다.

- **한 줄 주석** ( Single Line Comments )

  - 한 줄 주석은 `--`로 시작한다.

  - `--` 와 줄 끝 사이의 모든 텍스트는 무시된다 (실행되지 않는다).

    > --Select all:
    > **SELECT** * **FROM** Customers;

  - 다음 예제에서는 한 줄 주석을 사용하여 줄 끝을 무시한다.

    > **SELECT** * **FROM** Customers -- **WHERE** City='Berlin';

  - 다음 예제에서는 한 줄 주석을 사용하여 `SELECT` 문을 무시한다.

    > -- SELECT * FROM Customers;
    >
    > **SELECT** * **FROM** Products;

- **여러 줄 주석** ( Multi-line Comments )

  - 여러 줄 주석은  `/*`로 시작하고  `*/`로 끝나고, `/*`와  ` */` 사이의 모든 텍스트는 무시된다.

  - 다음 예에서는 설명으로 여러 줄 주석을 사용한다.

    > /* Select all the columns
    > of all the records
    > in the Customers table: */
    > **SELECT** * **FROM** Customers;

  - 다음 예에서는 여러 줄 주석을 사용하여 많은 구문을 무시한다.

    > /*** SELECT** * **FROM** Customers;
    > **SELECT** * **FROM** Products;
    > **SELECT** * **FROM** Orders;
    > **SELECT** * **FROM** Categories; */
    > **SELECT** * **FROM** Suppliers;

  - 명령문의 일부만 무시하려면 `/* */` 주석을 사용할 수 있다.

    > **SELECT** CustomerName, /* City, */ Country **FROM** Customers;

  - 다음 예에서는 주석을 사용하여 구문의 일부를 무시한다.

    > **SELECT** * **FROM** Customers **WHERE** (CustomerName **LIKE** 'L%'
    > **OR** CustomerName **LIKE** 'R%' /* OR CustomerName **LIKE** 'S%'
    > **OR** CustomerName **LIKE** 'T%' */ **OR** CustomerName **LIKE** 'W%')
    > **AND** Country='USA'
    > **ORDER BY** CustomerName;



# References

- [SQL Comments - w3schools.com](https://www.w3schools.com/sql/sql_comments.asp) 
