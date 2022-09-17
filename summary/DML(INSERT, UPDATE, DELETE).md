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
</br>
    
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
    - #### SubQuery Sample SQL
        ```SQL
        -- 방명수 사원의 급여, 보너스를 유재식 사원의 급여와 동일하게 수정
        UPDATE EMPLOYEE2 SET
        SALARY = (SELECT SALARY FROM EMPLOYEE WHERE EMP_NAME = '유재식'),
        BONUS = (SELECT BONUS FROM EMPLOYEE2 WHERE EMP_NAME = '유재식')
        WHERE EMP_NAME = '방명수';
        ```  
        </br>

## DELETE 
> ### 테이블의 행을 삭제하는 구문
> ### WHERE 절을 사용하여 삭제할 행을 특정
```SQL
DELETE FROM 테이블명 WHERE 조건설정
```

- ### Sample SQL
    ```SQL
    -- EMPLOYEE 테이블에서 이름이 '장채현'인 사원 정보 삭제
    DELETE FROM EMPLOYEE
    WHERE EMP_NAME = '장채현';
    ```

- ### DELETE 주의점
    > #### WHERE절 조건을 작성하지 않으면 테이블의 모든행이 삭제됨
    ```SQL
    DELETE FROM EMPLOYEE
    ```
    <h4><details>
    <summary>DELETE SQL 수행 전</summary>

    ![DELETE Example](../images/DML/DELETE%20Example.png);
    </details></h4> 

    <h4><details>
    <summary>DELETE SQL 수행 후</summary>

    ![DELETE Example2](../images/DML/DELETE%20Example2.png);
    </details></h4>  
    </br>

## MERGE 
> ### 구조가 같은 두 개의 테이블을 하나로 합치는 기능.
> ### 테이블에서 지정하는 조건의 값이 존재하면 **UPDATE** / 없으면 **INSERT**
```SQL
MERGE INTO EMP_M01 USING EMP_M02 ON(EMP_M01.EMP_ID = EMP_M02.EMP_ID)
WHEN MATCHED THEN
UPDATE SET
EMP_M01.EMP_NAME = EMP_M02.EMP_NAME,
EMP_M01.EMP_NO = EMP_M02.EMP_NO,
EMP_M01.EMAIL = EMP_M02.EMAIL,
EMP_M01.PHONE = EMP_M02.PHONE,
EMP_M01.DEPT_CODE = EMP_M02.DEPT_CODE,
EMP_M01.JOB_CODE = EMP_M02.JOB_CODE,
EMP_M01.SAL_LEVEL = EMP_M02.SAL_LEVEL,
EMP_M01.SALARY = EMP_M02.SALARY,
EMP_M01.BONUS = EMP_M02.BONUS,
EMP_M01.MANAGER_ID = EMP_M02.MANAGER_ID,
EMP_M01.HIRE_DATE = EMP_M02.HIRE_DATE,
EMP_M01.ENT_DATE = EMP_M02.ENT_DATE,
EMP_M01.ENT_YN = EMP_M02.ENT_YN
WHEN NOT MATCHED THEN
INSERT VALUES (EMP_M02.EMP_ID, EMP_M02.EMP_NAME, EMP_M02.EMP_NO,
               EMP_M02.EMAIL, EMP_M02.PHONE, EMP_M02.DEPT_CODE,
               EMP_M02.JOB_CODE, EMP_M02.SAL_LEVEL, EMP_M02.SALARY,
               EMP_M02.BONUS, EMP_M02.MANAGER_ID, EMP_M02.HIRE_DATE,
               EMP_M02.ENT_DATE, EMP_M02.ENT_YN);
```

