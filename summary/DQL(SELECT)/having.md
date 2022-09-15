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

