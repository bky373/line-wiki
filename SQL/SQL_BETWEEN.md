# SQL

## BETWEEN 연산자

  - `BETWEEN` 연산자는 **주어진 범위** 내에서 값을 선택한다. 값은 **숫자, 텍스트 또는 날짜** 일 수 있다.

  - `BETWEEN` 연산자는 **`inclusive`** 하다. 따라서 **시작 값과 마지막 값이 포함** 된다.

    > **SELECT**  column_name(s)
    >
    > **FROM**  table_name
    >
    > **WHERE**  column_name  **BETWEEN**  value1  **AND** value2;
    >
    > (실사용 예시)
    >
    > **SELECT**  *
    >
    > **FROM**  Products
    >
    > **WHERE**  Price **BETWEEN** 10 **AND** 20;

  - **NOT BETWEEN 예시**

    > **SELECT**  *
    >
    > **FROM**  Products
    >
    > **WHERE**  Price **NOT BETWEEN** 10 **AND** 20;

  - **BETWEEN** 과  **IN** 예시

    > **SELECT**  *
    >
    > **FROM**  Products
    >
    > **WHERE**  Price **BETWEEN** 10 **AND** 20
    >
    > **AND** CategoryID **NOT IN** (1, 2, 3);

  - **BETWEEN 텍스트 값 예시**

    - 다음 SQL 문은  `Carnarvon Tigers`와  `Chef Anton's Cajun Seasoning` 사이의 `ProductName` 값을 갖는 모든 레코드를 반환한다. 

    > **SELECT** * 
    >
    > **FROM** Products
    >
    > **WHERE** ProductName **BETWEEN** "Carnarvon Tigers" **AND** "Chef Anton's Cajun Seasoning"
    > **ORDER BY** ProductName;

    ![image](https://user-images.githubusercontent.com/49539592/127869665-09f0f3a4-fdc8-4f86-b0e7-8e6cd96d1f9a.png)

  - **BETWEEN 날짜 예시**

    - 다음 SQL 문은  `OrderDate`가  `'01-July-1996'` 과  `'31-July-1996'`  사이에 있는 모든 주문을 선택한다.

      > **SELECT** *
      >
      > **FROM** Orders
      >
      > **WHERE** OrderDate **BETWEEN** \#07/01/1996# **AND** #07/31/1996#;

      또는

      > **SELECT** *
      >
      > **FROM** Orders
      >
      > **WHERE** OrderDate
      >
      > **BETWEEN** `1996-07-01` **AND**  `1996-07-31`;
      

# References

- [SQL Between - w3schools.com](https://www.w3schools.com/sql/sql_between.asp) 
