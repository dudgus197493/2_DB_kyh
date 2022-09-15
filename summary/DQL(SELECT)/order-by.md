

## ORDER BY
> ### SELECT문의 조회 결과(RESULT SET)를 정렬할 때 사용되는 구문
> *SELECT문 해석 시 가장 마지막에 해석*
```SQL
ORDER BY 컬럼명 | 별칭 | 컬럼 순서 [ASC | DESC] [NULL FIRST | LAST]
```
- ### Sample SQL
```SQL
-- EMPLOYEE 테이블 급여 오름 차순으로
-- 사번, 이름, 급여 조회
SELECT EMP_ID, EMP_NAME, SALARY
FROM EMPLOYEE
ORDER BY SALARY; -- ASC 기본값

-- 급여 200만 이상인 사원의
-- 사번, 이름, 급여
-- 급여 많은 순으로 조회
SELECT EMP_ID, EMP_NAME, SALARY
FROM EMPLOYEE
WHERE SALARY >= 2000000
ORDER BY 3 DESC; -- 컬럼 순서로 지정 가능 (가장 마지막에 해석되기 때문 )
```
- ### 정렬 중첩
> #### 대분류 정렬 후 소분류 정렬
```SQL
-- 부서코드 오름차순 정렬 후 급여 내림차순 정렬
SELECT EMP_NAME, DEPT_CODE, SALARY
FROM EMPLOYEE
ORDER BY DEPT_CODE, SALARY DESC;
```