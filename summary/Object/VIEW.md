# VIEW
> ### SELECT문의 실행 결과(RESULT SET)을 저장하는 객체
> ### 논리적 가상 테이블 (테이블 모양을 하지만 실제로 값을 저장 X)  
```SQL
CREATE [OR REPLACE] [FORCE | NOFORCE] VIEW 뷰이름 [(alias[, alias]...)]
AS subquery
[WITH CHECK OPTION]
[WITH READ ONLY];
```
</br>

## VIEW 옵션
|**구분**|**기능**|
|:--:|:--|
|OR REPLACE|기존에 동일한 뷰 이름이 존재하는 경우 **덮어쓰기**</br>존재하지 않으면 **새로 생성**|
|FORCE &#124; NOFORCE|FORCE : 서브쿼리에 사용된 테이블이 존재하지 않아도 뷰 생성</br>NOFORCE : 서브쿼리에 사용된 테이블이 존재해야만 뷰 생성(기본값)
|WITH CHECK OPTION|웁션을 설정한 컬럼의 값을 수정 불가능|
|**WITH READ ONLY**|뷰에 대해 조회만 가능(DML 수행 불가)|
  
</br>

## VIEW Sample SQL
```SQL
CREATE VIEW V_EMP
AS SELECT EMP_ID, EMP_NAME, DEPT_TITLE, JOB_NAME
FROM EMPLOYEE
JOIN DEPARTMENT ON (DEPT_ID = DEPT_CODE)
JOIN JOB USING(JOB_CODE);

CREATE VIEW V_EMP -- ORA-00955: 기존의 객체가 이름을 사용하고 있습니다. => 이름 중복
AS SELECT EMP_ID, EMP_NAME, DEPT_TITLE, JOB_NAME
FROM EMPLOYEE
JOIN DEPARTMENT ON (DEPT_ID = DEPT_CODE)
JOIN JOB USING(JOB_CODE);

CREATE OR REPLACE VIEW V_EMP -- OR REPLACE 추가 후 성공 (기존 V_EMP VIEW에 새로운 V_EMP 덮어쓰기)
AS SELECT EMP_ID, EMP_NAME, DEPT_TITLE, JOB_NAME
FROM EMPLOYEE
JOIN DEPARTMENT ON (DEPT_ID = DEPT_CODE)
JOIN JOB USING(JOB_CODE);
```
</br>

## VIEW 사용 목적
1. #### 복잡한 SELECT문을 쉽게 **재사용**하기 위해 사용
2. #### 테이블의 진짜 모습을 감출 수 있어 보안상 유리
  
</br>

## VIEW 주의 사항
1. #### 가상의 테이블 -> ALTER 구문 사용 불가
2. #### VIEW를 이용한 DML 사용시 원본 테이블에 진행
3. #### VIEW를 이용한 INSERT 사용시 VIEW에 포함되지 않은 컬럼에는 NULL 삽입
4. #### 만약 VIEW에 포함되지 않은 컬럼에 제약조건이 설정되어 있는경우 정상적인 INSERT 진행 X
5. #### VIEW를 이용한 DML(INSERT, UPDATE, DELETE)가 가능한 경우도 있지민 **많은 제약이 따르기 때문에 SELECT 용도로 사용 권장**


