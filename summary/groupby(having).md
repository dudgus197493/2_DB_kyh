# GROUP BY 절
> ### 같은 값들이 여러개 기록된 컬럼을 가지고 **같은 값들을 하나의 그룹**으로 묶음
> ### 여러개의 값을 묶어서 하나로 처리할 목적으로 사용
> ### 그룹으로 묶은 값에 대해서 SELECT절에서 그룹함수를 사용함
> *그룹 함수는 단 한개의 결과 값만 산출하기 때문에 그룹이 여러 개일 경우 오류 발생*  
> *여러 개의 결과 값을 산출하기 위해 그룹 함수가 적용된 그룹의 기준을 ORDER BY절에 기술하여 사용*
```SQL
GROUP BY 컬럼명 | 함수식, ...
```

## GROUP BY Sample SQL
```SQL
-- EMPLOYEE 테이블에서 부서코드, 부서별 급여 합 조회
SELECT DEPT_CODE, SUM(SALARY)
FROM EMPLOYEE; -- ORA-00937: 단일 그룹의 그룹 함수가 아닙니다

SELECT DEPT_CODE, SUM(SALARY)
FROM EMPLOYEE
GROUP BY DEPT_CODE; -- DEPT_CODE가 같은 행 끼리 하나의 그룹이 됨

-- EMPLOYEE 테이블에서
-- 직급코드가 같은 사람의 직급코드, 급여 평군, 인원 수를
-- 직급코드 오름 차순으로 조회
SELECT JOB_CODE, AVG(SALARY), COUNT(*)
FROM EMPLOYEE
GROUP BY JOB_CODE
ORDER BY JOB_CODE;
```

## GROUP BY 절 함수 사용
```SQL
-- EMPLOYEE 테이블에서
-- 성별과 각 성별 별 인원 수, 급여 합을
-- 인원 수 내림차순으로 조회
SELECT DECODE(SUBSTR(EMP_NO, 8, 1), '1', '남', '여'), COUNT(*) "인원 수", SUM(SALARY) "급여 합"
FROM EMPLOYEE
GROUP BY DECODE(SUBSTR(EMP_NO, 8, 1), '1', '남', '여')
ORDER BY "인원 수";
```

## WHERE가 GROUP BY 혼합
> ### WHERE 조건에 맞는 행중에 같은 값을 가진 컬럼을 그룹으로 지정
```SQL
-- EMPLOYEE 테이블에서 부서코드가 D5, D6인 부서의 평균 급여, 인원 수 조회
SELECT DEPT_CODE, ROUNT(AVG(SALARY)), COUNT(*)
FROM EMPLOYEE
WHERE DEPT_CODE IN ('D5', 'D6')
GROUP BY DEPT_CODE;

-- EMPLOYEE 테이블에서 직급 별 2000년도 이후 입사자들의 급여 합을 조회
SELECT JOB_CODE, SUM(SALARY)
FROM EMPLOYEE
WHERE EXTRACT(YEAR FROM HIRE_DATE) >= 2000
GROUP BY JOB_CODE
ORDER BY JOB_CODE;
```

## 그룹 중첩 (그룹내 그룹)
```SQL
-- EMPLOYEE 테이블에서 부서별로 같은 직급인 사원의 수를 조회
-- (부서코드 오름차순, 직급코드 내립차순)
SELECT DEPT_CODE, JOB_CODE, COUNT(*)
FROM EMPLOYEE
GROUP BY DEPT_CODE, JOB_CODE -- DEPT_CODE로 그룹을 나누고, 나눠진 그룹 내에서 JOB_CODE로 또 그룹화
ORDER BY DEPT_CODE, JOB_CODE DESC;
```

## GROUP BY절 주의점
> ### SELECT문에 GROUP BY절을 사용할 경우 SELECT절에 명시한 컬럼중
> ### ***그룹함수가 적용되지 않은 컬럼을 모두 GROUP BY절에 작성***
```SQL
SELECT DEPT_CODE, JOB_CODE, COUNT(*)
FROM EMPLOYEE
GROUP BY DEPT_CODE -- ORA-00979: GROUP BY 표현식이 아닙니다.
--> SELECT절에 그룹함수가 적용되지 않은 컬럼중 JOB_CODE가 GROUP BY절에 작성되지 않음

-- 수정한 SQL
SELECT DEPT_CODE, JOB_CODE, COUNT(*)
FROM EMPLOYEE
GROUP BY DEPT_CODE, JOB_CODE;
```  
</br>

# HAVING 절
> ### 그룹함수로 구해 올 그룹에 대한 조건을 설정
> *GROUP BY로 만든 그룹중 조건에 맞는 그룹만 조회*
```SQL
HAVING 컬럼명 | 함수식 비교연산자 비교값
```

## HAVING Sample SQL
```SQL
-- 부서별 평균 급여가 3백만원 이상인 부서를 조회(부서코드 오름차순)
SELECT DEPT_CODE
FROM EMPLOYEE
GROUP BY DEPT_CODE  -- 같은 부서끼리 그룹을 나눔
HAVING AVG(SALARY) >= 3000000 -- 나눈 그룹중 평균급여가 3백만원 이상인 부서 조회
ORDER BY DEPT_CODE;

-- EMPLOYEE 테이블에서 직급별 인원수가 5명 이하인 직급코드, 인원수 조회(직급코드 오름차순)
SELECT JOB_CODE, COUNT(*)
FROM EMPLOYEE
GROUP BY JOB_CODE -- 같은 직급끼리 그룹을 나눔
HAVING COUNT(*) <= 5 -- 나눈 그룹중 인원수가 5명이하인 그룹만 조회
ORDER BY JOB_CODE;
```

## WHERE과 GROUP BY의 차이점
- ### WHERE
```SQL
SELECT DEPT_CODE
FROM EMPLOYEE
WHERE SALARY >= 3000000 --> 급여가 3백만이상인 사람중에
GROUP BY DEPT_CODE  -- 같은 부서끼리 그룹화
ORDER BY DEPT_CODE;
```
- ### HAVING
```SQL
SELECT DEPT_CODE
FROM EMPLOYEE
GROUP BY DEPT_CODE  -- 같은 부서끼리 그룹화
HAVING AVG(SALARY) >= 3000000 -- 그룹화 된 사람들의 평균 급여가 3백만 이상인 그룹만 조회
ORDER BY DEPT_CODE;
```  
</br>

## 집계함수(ROLLUP, CUBE)
> ### 그룹 별 산출 결과 값의 집계를 계산하는 함수
> 그룹별로 중간 집계 결과를 추가  
> *GROUP BY 절에서만 사용할 수 있는 함수*

- ### ROLLUP
    > #### GROP BY 절에서 가장 먼저 작성된 컬럼의 중간 집계를 처리하는 함수
    ```SQL
    SELECT DEPT_CODE, JOB_CODE, COUNT(*)
    FROM EMPLOYEE
    GROUP BY ROLLUP(DEPT_CODE, JOB_CODE) -- DEPT_CODE가 같은 그룹의 중간집계를 계산
    ORDER BY 1;
    ```

- ### CUBE
    > #### GROUP BY 절에 작성된 모든 컬럼의 중간 집계를 처리하는 함수
    ```SQL
    SELECT DEPT_CODE, JOB_CODE, COUNT(*)
    FROM EMPLOYEE
    GROUP BY CUBE(DEPT_CODE, JOB_CODE) -- DEPT_CODE가 같은 그룹의 중간집계 계산 후, 
                                    -- JOB_CODE가 같은 그룹의 중간집계도 계산
    ORDER BY 1;
    ```
