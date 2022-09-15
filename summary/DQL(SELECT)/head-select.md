# SELECT문
> ### 데이터를 조회(SELECT)하면 조건에 맞는 행들이 조회됨.
> *조회된 행들의 집합을 "RESULT SET" (조회 결과의 집합) 이라고 한다.*

## SELECT문 해석 순서

|해석순서|작성|
|--:|:--:|
|5|[SELECT](select.md)|
|1|[FROM(JOIN)](join.md)|
|2|[WHERE](where.md)|
|3|[GROUP BY](group-by(having).md)|
|4|[HAVING](having.md)|
|6|[ORDER BY](order-by.md)|

## 연산자 우선 순위

|우선순위|구분|
|--:|:--:|
|1|산술연산자 ( + - * / )|
|2|연결연산자 ( &#124;&#124; )|
|3|비교연산자 ( > < >= <= = != <> )|
|4|IS NULL / IS NOT NULL, LIKE, IN / NOT IN|
|5|BETWEEN AND / NOT BETWEEN AND
|6|NOT(논리연산자)|
|7|AND(논리연산자)|
|8|OR(논리연산자)|


