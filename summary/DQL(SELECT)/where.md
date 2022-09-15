# WHERE 절
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

- ## 기본
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
  
- ## 여러개의 조건 사용
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

- ## BETWEEN A AND B
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
-- EMPLOYEE 테이블에서 전화번호가 010으로 시작하는 사원의
-- 사번, 이름, 전화번호 조회
SELECT EMP_ID, EMP_NAME, PHONE
FROM EMPLOYEE
WHERE PHONE LIKE '010________';
```

- ### ESCAPE
```SQL
-- EMAIL에서 _앞에 글자가 세 글자인 사원 조회
SELECT EMP_NAME, EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE '____%'; -- 와일드카드 _과 리터럴 _이 구분되지 않음

-- ESCAPE 사용해 해결
SELECT EMP_NAME, EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE '___#_%' ESCAPE '#';
-- #뒤에 있는 _는 문자열취급
```  
</br>

## WHERE - IN
> ### 비교하려는 값과 목록에 작성된 값 중 일치하는 것이 있으면 조회
```SQL
WHERE 컬럼명 IN (값1, 값2, 값3, ...)
-- 위와 같은 구문
WHERE 컬럼명 = 값1
OR 컬럼명 = 값2
OR 컬럼명 = 값3
...
```

- ### IN
```SQL
-- EMPLOYEE 테이블에서
-- 부서코드가 D1, D6, D7인 사원의
-- 사번, 이름, 부서코드 조회
SELECT EMP_ID, EMP_NAME, DEPT_CODE
FROM EMPLOYEE
WHERE DEPT_CODE IN ('D1', 'D6', 'D7');
```

- ### NOT IN
```SQL
-- EMPLOYEE 테이블에서
-- 부서코드가 D1, D6, D7가 아닌 사원의
-- 사번, 이름, 부서코드 조회
SELECT EMP_ID, EMP_NAME, DEPT_CODE
FROM EMPLOYEE
WHERE DEPT_CODE NOT IN ('D1', 'D6', 'D7');
```  
</br>

- ### NULL 처리 연산자
> #### Java에서 NULL : 참조하는 객체가 없음을 의미하는 값
> #### DB에서 NULL : 컬럼에 값이 없음을 의미하는 값
> *IS NULL : NULL인 경우 조회*  
> *IS NOT NULL : NULL이 아닌 경우 조회*
```SQL
-- EMPLOYEE 테이블에서 보너스가 있는 사원의 이름, 보너스 조회
SELECT EMP_NAME, BONUS
FROM EMPLOYEE
WHERE BONUS IS NOT NULl;

-- EMPLOYEE 테이블에서 보너스가 없는 사원의 이름, 보너스 조회
SELECT EMP_NAME, BONUS
FROM EMPLOYEE
WHERE BONUS IS NULL;
```