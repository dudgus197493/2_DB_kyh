# SELECT 
> ### ResultSet : 데이터를 조회한 결과, SELECT구문에 의해 **조회된 행들의 집합** 을 의미.
> ### ResultSet은 **0개 이상**의 행이 포함될 수 있다.
> ### ResultSet은 특정한 기준에 의해 정렬 가능
> ### 한 테이블의 특정 컬럼, 특정 행, 특정 행/컬럼 또는 여러 테이블의 특정 행/ 컬럼 조회 가능

## 기본 
```SQL
SELECT 컬렴명 [, 컬럼명, ...]
FROM 테이블명;
```
- ### SELECT : 
   - 조회하고자 하는 컬럼명 기술  
   - 여러 컬럼을 조회하는 경우 컬름은 쉼표로 구분, 마지막 컬럼 다음은 쉼표를 사용하지 않음  
   - 모든 컬럼 조회시 컬럼 명 대신 '*' 기호 사용 가능하며 조회 결과는 기술한 컬럼 명 순으로 표시 됨

- ### FROM :
  - 조회 대상 컬럼이 포함된 테이블 명 기술
```SQL
-- 위 규칙을 지킨 샘플 SQL
-- SampleSQL 1
SELECT EMP_ID, EMP_NAME, PHONE 
FROM EMPLOYEE;
--> EMPLOYEE 테이블에서 EMP_ID, EMP_NAME, PHONE 컬럼 조회

-- SampleSQL 2
SELECT * FROM EMPLOYEE
--> EMPLOYEE 테이블에서 모든 컬럼 조회
```  
</br>

## 컬럼 값 산술 연산
> ### 컬럼 값 : 테이블 내 한칸(== 한 셀)에 작성된 값(DATA)
> ### 산술 연산은 **숫자(NUMBER)** 타입만 가능하다
```SQL
SELECT EMP_ID, EMP_NAME, SALARY, SALARY * 12 -- 연봉 (급여 * 12달)
FROM EMPLOYEE;

SELECT EMP_NAME * 10
FROM EMPLOYEE;
-- ORA-01722: 수치가 부적합합니다.
-- 산술 연산은 숫자(NUMBER)타입만 가능하다.
```  
</br>

## 날짜(DATE) 타입 조회
> ### SYSDATE : 시스템상의 현재 시간(날짜) 나타내는 함수
> ### DATE타입도 산술연산 가능
```SQL
-- 현재 시간 조회
SELECT SYSDATE
FROM DUAL; -- DUAL(DUmmy tAbLe) 테이블 : 가짜 테이블(임시 조회용 테이블)

-- 날짜 + 산술 연산(+, -)
SELECT SYSDATE - 1, SYSDATE, SYSDATE + 1
FROM DUAL;
--> 날짜의 +/- 연산 시 일 단위로 계산이 진행됨.
--> SYSDATE - 1 : 어제
--> SYSDATE + 1 : 내일
```  
</br>

## 컬럼 별칭 지정
> ### SELECT 조회 결과의 집합인 RESULT SET에 출력되는 **컬럼명을 지정**
> ### 컬럼명 AS 별칭 : 띄어쓰기X, 특수문자X, 문자만O  
> ### 컬럼명 AS "별칭" : 띄어쓰기, 특수문자O, 문자만O
> *AS는 **생략가능***
```SQL
SELECT SYSDATE-1 "하루 전", SYSDATE AS 현재시간, SYSDATE+1 내일
FROM DUAL;
```  
</br>

## 리터털
> ### 임의로 지정한 값을 SELECT절에 사용하여 **기존 테이블에 존재하는 값 처럼** 사용하는 것
> ### ' ' : DB의 기본적인 리터럴 표기법
> ### " " : 특수문자, 대소문자, 기호 등을 구분하여 나타낼 때 사용, 쌍따옴표 안에 작성되는 것들이 '하나'임을 의미
```SQL
SELECT EMP_NAME, SALARY, '원 입니다.'
FROM EMPLOYEE;
```
||EMP_NAME|SALARY|원 입니다.|
|:--:|:--:|:--:|:--|
|1|홍길동|3000000|원 입니다.|
|2|아이언맨|4000000|원 입니다.|
|3|스파이더맨|6000000|원 입니다.|
|...|

## DISTINCT 
> ### 조회 시 컬럼에 포함된 중복 값을 한 번만 표기
```SQl
SELECT DISTINCT DEPT_CODE, JOB_CODE
FROM EMPLOYEE;
```
- ### 작성 시 주의 사항
    1. `DISTINCT` 구문은 `SELECT` 마다 **딱 한번씩만 작성** 가능.
    2. `DISTINCT` 구문은 `SELECT` **제일 앞에 작성**되어야 한다.
    3. 둘 이상의 컬럼앞에 `DISTINCT` 튜플 전체 값이 동일한 경우에 한해서 중복 제거

## WHERE 절
> ### 검색할 컬럼의 **조건을 설정**하여 행 결정
> ### 비교연산자 (>, <, >=, <=, =, !=, <>) 사용 가능
> ### 여러개의 조건을 사용할때 논리 연산자 (AND, OR) 사용
> ### 범위를 조건으로 지정할때 `BETWEEN A AND B` 사용 가능
> ### 논리부정 `NOT` 사용 가능
- ### 해석순서
  |3.|SELECT 절| SELECT 컬럼명
  |:--:|:--:|:--|
  |1.|FROM 절| FROM 테이블명
  |2.|WHERE (조건절)| WHERE 컬럼명 연산자 값

- ### 기본
```SQL
-- EMPLOYEE 테이블에서 급여가 3백만원 초과인 사원이
-- 사번, 이름, 급여, 부서코드를 조회
SELECT EMP_ID, EMP_NAME, DEPT_CODE
FROM EMPLOYEE
WHERE SALARY > 3000000;

-- EMPLOYEE 테이블에서 부서코드가 'D9'인 사원의
-- 사번, 이름, 부서코드, 직급코드 조회
SELECT EMP_ID, EMP_NAME, DEPT_CODE, JOB_CODE
FROM EMPLOYEE
WHERE DEPT_CODE = 'D9';
```
  
- ### 여러개의 조건 사용
```SQL
-- EMPLOYEE 테이블에서 급여가 300만 미만 또는 500만 이상인 사원의
-- 사번, 이름, 급여, 전화번호 조회
SELECT EMP_ID, EMP_NAME, SALARY, PHONE
FROM EMPLOYEE
WHERE SALARY < 3000000 OR SALARY >= 5000000;

-- EMPLOYEE 테이블에서 급여가 300 이상이고 500만 미만인 사원의
-- 사번, 이름, 급여, 전화번호 조회
SELECT EMP_ID, EMP_NAME, SALARY, PHONE
FROM EMPLOYEE
WHERE SALARY >= 3000000 AND SALARY < 5000000;
```

- ### BETWEEN A AND B
```SQL
-- 300만 이상 600만 이하
SELECT EMP_ID, EMP_NAME, SALARY
FROM EMPLOYEE
WHERE SALARY BETWEEN 3000000 AND 6000000;

-- EMPLOYEE 테이블에서 입사일이 1990-01-01 ~ 1999-12-31사이인 직원
SELECT EMP_NAME, HIRE_DATE
FROM EMPLOYEE
WHERE HIRE_DATE BETWEEN '1990-01-01' AND '1999-12-31';
-- 오라클은 데이터 타입이 달라도 형태가 일치하면 자동으로 타입을 변환시킴
-- ex) 1 == '1'
SELECT '같음' FROM DUAL WHERE 1 = '1';   -- 같음 출력
SELECT '다름' FROM DUAL WHERE NOT 1 = 2; -- 다름 출력
```

## WHERE - LIKE
> ### 비교하려는 값이 특정한 패턴을 만족 시키면 조회하는 연산자
> ### LIKE뒤 패턴으로 (%, _) 와일드카드 작성
```SQL
WHERE 컬럼명 LIKE '패턴이 적용된 값' 
```

- ### `%` 와일드카드
|구분|설명|
|:--:|:--|
|`A%`|문자열이 A로 시작하는 문자열|
|`%A`|A로 끝나는 문자열|
|`%A%`|A를 포함하는 문자열|
```SQL
-- EMP_NAME이 김으로 시작하거나 현으로 끝나거나
-- 영이 포함되는 사람 조회
SELECT EMP_NAME
FROM EMPLOYEE
WHERE EMP_NAME LIKE '김%'
OR EMP_NAME LIKE '%현'
OR EMP_NAME LIKE `%영%`;
```

- ### `_` 와일드카드
|구분|설명|
|:--:|:--|
|`A_`|A로 시작하는 두 글자 문자열|
|`___A`|A로 끝나는 네 글자 문자열|
|`__A__`|세 번째 문자가 A인 다섯 글자 문자열|
|`_____`|다섯 글자 문자열|
```SQL

```




  

