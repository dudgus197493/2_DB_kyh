# DML(Data Manipulation Language)
> ### 데이터 조작 언어
> ### 테이블에 값을 삽입(INSERT), 수정(UPDATE), 삭제(DELETE) 하는 SQL

## INSERT
> ### 테이블에 새로운 행을 추가하는 구문

- ### 작성법 1
    ```SQL
    INSERT INTO 테이블명 VALUES (데이터 [, 데이터 ...])
    ```
    > #### 테이블의 **모든 컬럼**에 대한 값을 INSERT  
    > #### *컬럼의 순서를 지켜 VALUES에 값을 기입*

    - #### Sample SQL
        ```SQL
        INSERT INTO EMPLOYEE2 
        VALUES ('900', '장채현', '901230-2345678', 'jang_ch@kh.or.kr', '01012341234'
        , 'D1', 'J7', 'S3', 4300000, 0.2, 200, SYSDATE, NULL, 'N'); --> 모든 컬럼에 대응하는 값 추가
        ```

- ### 작성법 2
    ```SQL
    INSERT INTO 테이블명 (컬럼명 [, 컬럼명]) VALUES (데이터 [, 데이터 ...])
    ```
    > #### 테이블에 내가 선택한 컬럼에 대한 값만 INSERT  
    > #### 선택안된 컬럼은 `NULL` 값이 삽입 (`DEFAULT` 존재 시 해당 값으로 삽입)  
    > #### *선택한 컬럼 갯수와 VALUES에 들어갈 데이터 갯수가 동일해야 함*
    > *작성법 1번 보다 속도적인 우위가 있음*

    - #### Sample SQL
        ```SQL
        INSERT INTO EMPLOYEE2 (EMP_ID, EMP_NAME, EMP_NO, EMAIL, PHONE, DEPT_CODE, JOB_CODE, SAL_LEVEL, SALARY) 
        VALUES('900', '장채현', '901123-2345678', 'jang_ch@kh.or.kr',   '01012341234',
       'D1', 'J7', 'S3', 4300000); -- 속도적인 우위가 있다.
        ```

- ### [서브쿼리]()를 활용한 INSERT
    > #### 서브쿼리 결과 (RESULT SET) 을 INSERT
    > #### 서브쿼리 결과의 데이터 타입, 컬럼 개수가 INSERT 하려는 테이블의 컬럼과 일치해야 함
    > *단, 데이터 타입 자동 형변환 적용*
    ```SQL
    INSERT INTO EMP_01 (
        SELECT EMP_ID, EMP_NAME, DEPT_TITLE
        FROM EMPLOYEE
        JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID)) --> EMP_01 테이블에 서브쿼리 결과를 INSERT
    ```

## UPDATE
> ### 테이블에 기록된 컬럼의 값을 수정하는 구문
> ### WHERE 절을 사용하여 수정할 컬럼의 위치를 특정
> ### 여러컬럼을 한번에 수정할 시 콤마(,)로 컬럼을 구분
```SQL
UPDATE 테이블명 SET 컬럼명 = 바꿀값 [WHERE 컬럼명 비교연산자 비교값];
```
- ### Sample SQL
    ```SQL
    -- DEPARTMENT 테이블에서 DEPT_ID가 'D9'인 행의 DEPT_TITLE을 '전략기획팀'으로 수정
    UPDATE DEPARTMENT 
    SET DEPT_TITLE = '전략기획팀'
    WHERE DEPT_ID = 'D9';

    -- EMPLOYEE 테이블에서 BONUS를 받지 않는 사원의 BONUS 0.1로 변경
    UPDATE EMPLOYEE
    SET BONUS = 0.1
    WHERE BONUS IS NULL;

    --＊여러컬럼을 한번에 수정할 시 콤마(,)로 컬럼을 구분하면됨.
    -- D9 / 전략기획팀 -> D0 / 전략기획2팀으로 수정
    UPDATE DEPARTMENT
    SET DEPT_ID = 'D0',
    DEPT_TITLE = '전략기획2팀'
    WHERE DEPT_ID = 'D9'
    AND DEPT_TITLE = '전략기획팀'
    ```
- ### UPDATE 주의점
    > #### WHERE절 조건을 작성하지 않으면 모든 행의 컬럼이 수정됨
    ```SQL
    UPDATE DEPARTMENT
    SET DEPT_TITLE = '기술연구팀';
    ```

    <h4><details>
    <summary>UPDATE 결과</summary>

    ![UPDATE Example](../images/DML/UPDATE%20Example.png);
    > *모든 행의 DEPT_TITLE 컬럼이 '기술연구팀'으로 변경*
    </details></h4> 

- ### [서브쿼리]()를 활용한 UPDATE
    ```SQL
    UPDATE 테이블명
    SET 컬럼명 = (서브쿼리)
    ```


