# SQL

## LIKE 연산자

`LIKE` 연산자는 `WHERE` 절에서 열에서 지정한 패턴을 검색하는 데 사용된다.

> **SELECT** column1, column2, ...
>
> **FROM**  table_name
>
> **WHERE** columnN  **LIKE** pattern;

- 아래 두 개의 **와일드카드**는  `LIKE` 연산자와  자주 사용된다.

  > **퍼센트 기호 ( `%` )** : 0, 1 또는 여러 개의 문자를 나타낸다.
  >
  > **밑줄 기호 ( `_` )** : 하나의 단일 문자를 나타낸다.
  >
  > - 참고로  `MS Access`는 퍼센트 기호 ( `%` )  대신 별표 ( `*` )를 사용하고,  밑줄 ( `_` ) 대신 물음표 ( `?` )를 사용한다.

- `LIKE` 연산자와  `%`  및  `_`  와일드카드의 사용 예

  | LIKE 연산자                               | 설명                                              |
  | ----------------------------------------- | ------------------------------------------------- |
  | **WHERE** CustomerName **LIKE** 'a%'      | `a`로 시작하는 모든 값을 찾는다.                  |
  | **WHERE** CustomerName **LIKE** '%a'      | `a`로 끝나는 모든 값을 찾는다.                    |
  | **WHERE** CustomerName **LIKE** '%or%'    | 위치에 상관없이 `or` 를 포함하는 값을 찾는다.     |
  | **WHERE** CustomerName **LIKE** '_r%'     | 두 번째 위치에  `r` 이 있는 값을 찾는다.          |
  | **WHERE** CustomerName **LIKE** 'a_%'     | `a`로 시작하고 길이가 2자 이상인 값을 찾는다.     |
  | **WHERE** CustomerName **LIKE** 'a__%'    | `a`로 시작하고 길이가 3자 이상인 값을 찾는다.     |
  | **WHERE** CustomerName **LIKE** 'a%z'     | `a`로 시작하고 `z`로 끝나는 값을 찾는다.          |
  | **WHERE** CustomerName **LIKE** '[ace]%'  | `a`, `c` 또는 `e` 로 시작하는 값을 찾는다.        |
  | **WHERE** CustomerName **LIKE** '[a-f]%'  | `a` 부터  `f` 사이의 문자로 시작하는 값을 찾는다. |
  | **WHERE** CustomerName **LIKE** '[!ace]%' | `a`, `c` 또는 `e` 로 시작하지 않는 값을 찾는다.   |


# References

- [SQL Like - w3schools.com](https://www.w3schools.com/sql/sql_like.asp) 
