# SQL

## **SELECT DISTINCT 문**

  - 중복값을 제거하고 컬럼의 **고유한(다른)  값** 을 얻기 위해 사용된다.

    > **SELECT  DISTINCT**   column1,  column2, ... 
    >
    > **FROM**   table_name;
    >
    > 
    >
    >
    > --  다음의 SQL 문은 고객 테이블에서 서로 다른(고유한) 국가의 수를 반환한다.
    >
    > **SELECT**  **COUNT**(**DISTINCT**  Country)  
    >
    > **FROM**  Customers; 
    >
    > --  결과
    >
    > 컬럼 이름 : COUNT(DISTINCT Country)  
    >
    > 값 : 21

    

    > --  위와 동일한 값을 얻으면서 동시에 컬럼의 이름을 변경한 쿼리는 다음과 같다.
    >
    > **SELECT**  **COUNT**(*)  **AS**  DistinctCountries
    >
    > **FROM**  (**SELECT  DISTINCT**  Country  **FROM**  Customers);
    >
    > --  결과
    >
    > 컬럼 이름 : DistinctCountries   
    >
    > 값 : 21



# References

- [SQL SELECT DISTINCT - w3schools.com](https://www.w3schools.com/sql/sql_distinct.asp) 
