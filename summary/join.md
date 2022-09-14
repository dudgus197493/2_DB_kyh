# JOIN 
> ### 관계형 데이터베이스에서 SQL을 이용해 테이블간 **'관계'** 를 맺는 방법
> ### 하나 이상의 테이블에서 데이터를 조회하기 위해 사용
> ### 수행 결과는 하나의 Result Set
> ### 테이블을 연결하기 위해 **같은 값을 저장하는 컬럼**을 사용
> *JOIN은 서로 다른 테이블의 행을 하나씩 이어 붙이기 때문에  
> **시간이 오래걸리는 단점이 있음***

## JOIN의 목적
- ### 기존에 서로 다른 테이블의 데이터를 조회 할 경우
    ```SQL
    -- 직원번호, 직원명, 부서코드, 부서명을 조회 하고자 할 때
    SELECT EMP_ID, EMP_NAME, DEPT_CODE
    FROM EMPLOYEE;
    --> 직원번호, 직원명, 부서코드는 EMPLOYEE 테이블에서 조회

    SELECT DEPT_TITLE
    FROM DEPARTMENT;
    --> 부서명은 DEPARTMENT 테이블에서 조회가능

    ---> 총 2번의 SELECT문 사용해 조회
    ```
- ### JOIN 사용 시
    ```SQL
    SELECT EMP_ID, EMP_NAME, DEPT_CODE, DEPT_TITLE
    FROM EMPLOYEE
    JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID);
    --> 조회하고자 하는 컬럼이 있는 테이블을 합쳐 한번의 SELECT문을 사용해 조회
    ```  
    </br>


## JOIN의 종류
1. ### 등가 조인
    - 내부 조인(`INNER JOIN`), `JOIN USNIG` / `ON`
    - 자연 조인(`NATURAL JOIN`, 등가 조인 방법 중 하나)
2. ### 포괄 조인
    - 왼쪽 외부 조인(`LEFT OUTER`)
    - 오른쪽 외부 조인 (`RIGHT OUTER`)
    - 전체 외부 조인(`FULL OUTER`, 오라클 구문으로는 사용 못함)
3. ### 자체 조인, 비등가 조인
4. ### 카테시안(카티션) 곱
    - 교차 조인(`CROSS JOIN`)
  
  </br>

## 내부 조인 (INNER JOIN)
> ### 연결되는 컬럼의 값이 **일치하는 행들만 조인**됨.
> *일치하는 값이 없는 행은 조인에서 제외됨*

- ### 연결에 사용할 컬럼명이 다른 경우 (OUTER JOIN도 동일)
    > EMPLOYEE 테이블, DEPARTMENT 테이블을 참조하여 사번, 이름, 직급코드, 직급명 조회

   - #### ANSI
    > `ON(컬럼명1 = 컬럼명2)` 을 사용
    ```SQL
    SELECT EMP_ID, EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE
    JOIN DEPARTMENT ON (DEPT_ID = DEPT_CODE)
    -- EMPLOYEE 테이블에 DEPT_CODE컬럼과 DEPARTMENT테이블에 DEPT_ID 컬럼은 서로 같은 부서 코드를 나타냄
    --> 이를 통해 두 테이블이 관계가 있음을 알고 조인을 통해데이터 추출
    ```
    - #### Oracle
    ```SQL
    SELECT EMP_ID, EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE, DEPARTMENT
    WHERE DEPT_CODE = DEPT_ID;
    ```
- ### 연결에 사용할 컬럼명이 같은 경우 (OUTER JOIN도 동일)
    > EMPLOYEE 테이블, JOB 테이블을 참조하여 사번, 이름, 직급코드, 직급명 조회

    - #### ANSI
    > `USING(컬럼명)` 을 사용
    ```SQL
    SELECT EMP_ID, EMP_NAME, JOB_CODE, JOB_NAME
    FROM EMPLOYEE
    JOIN JOB USING(JOB_CODE);

    -- ON()으로도 조인할 수 있음
    -- 단, 앞에 테이블명(테이블 별칭)을 명시해야함
    SELECT EMP_ID, EMP_NAME, JOB_CODE, JOB_NAME
    FROM EMPLOYEE
    -- JOIN JOB ON (JOB_CODE = JOB_CODE) -> ORA-00918: 열의 정의가 애매합니다.
    JOIN JOB ON (EMPLOYEE.JOB_CODE = JOB.JOB_CODE)
    ```

    - #### Oracle
    > 테이블명(별칭) 사용해 컬럼 출처 구분
    ```SQL
    SELECT EMP_ID, EMP_NAME, E.JOB_CODE, JOB_NAME
    FROM EMPLOYEE E, JOB J
    WHERE E.JOB_CODE = J.JOB_CODE;
    ```

- ### 내부 조인(INNER JOIN) 시 주의점
> #### 연결에 사용된 컬럼의 값이 일치하지 않으면 조회 결과에 포함되지 않음
```SQL
SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
FROM EMPLOYEE
JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID);
```
<h4><details>
    <summary>조회 결과</summary>

![INNER JOIN Example](images/INNER%20JOIN%20Example.png)
> ### `DEPT_CODE = DEPT_ID`를 하는 과정에서 EMPLOYEE 테이블의 DEPT_CODE가 NULL인 행(하동운, 이오리)은 JOIN에서 제외 했다.
</details></h4>

<h4><details>
    <summary>EMPLOYEE 테이블의 행의 갯수</summary>

![INNER JOIN Example2](images/INNER%20JOIN%20Example2.png)

</details></h4>
  
</br>


## 외부 조인(OUTER JOIN)
> ### 연결에 사용하는 컬럼값이 일치하지 않는 행도 조인

- ### LEFT [OUTER] JOIN
    > #### 연결에 사용한 두 테이블 중 왼편에 기술된 테이블의 컬럼 수를 기준으로 JOIN
    > *왼편에 작성된 테이블의 **모든 컬럼이 결과에 포함**되어야 함*  

    - #### ANSI
    ```SQL
    SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE
    LEFT JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID); -- EMPLOYEE 테이블의 모든 행이 결과에 포함
    ```

    - #### Oracle
    > 반대편 테이블 컬럼에 (+) 기호 작성
    ```SQL
    SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE, DEPARTMENT
    WHERE DEPT_CODE = DEPT_ID(+);
    ```

    <h4><details>
        <summary>조회 결과</summary>

    ![LEFT OUTER JOIN Example](images/LEFT%20OUTER%20JOIN%20Example.png)
    > ### DEPT_CODE와 DEPT_ID가 같지 않아도 EMPLOYEE 테이블의 모든행을 무조건 JOIN에 포함

    </details></h4>
    
- ### RIGHT [OUTER] JOIN
    > #### 연결에 사요안 두 테이블 중 오른편에 기술된 테이블의 컬럼 수를 기준으로 JOIN
    > *오른편에 작성된 테이블의 **모든 컬럼이 결과에 포함**되어야 함*

    - #### ANSI
    ```SQL
    SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE
    RIGHT JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID); -- DEPARTMENT 테이블의 모든 행이 결과에 포함
    ```

    - #### Oracle
    > 반대편 테이블 컬럼에 (+) 기호 작성
    ```SQL
    SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE, DEPARTMENT
    WHERE DEPT_CODE(+) = DEPT_ID;
    ```

    <h4><details>
        <summary>조회 결과</summary>

    ![RIGHT OUTER JOIN Example](images/RIGHT%20OUTER%20JOIN%20Example.png)
    > ### DEPT_ID와 DEPT_CODE가 같지 않아도 DEPARTMENT 테이블의 모든행을 무조건 JOIN에 포함

    </details></h4>
    
    

- ### FULL [OUTER] JOIN
    > #### 연결에 사용한 두 테이블이 가진 모든 행을 결과에 포함
    > *오라클 구문은 FULL OUTER JOIN을 사용 못함*

    - #### ANSI
    ```SQL
    SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE
    FULL JOIN DEPARTMENT ON (DEPT_CODE = DEPT_ID);
    ```
    <h4><details>
        <summary>조회 결과</summary>

    ![FULL OUTER JOIN Example](images/FULL%20OUTER%20JOIN%20Example.png))
    > ### DEPT_ID와 DEPT_CODE가 같지 않아도 두 테이블의 모든행이 무조건 JOIN에 포함

    </details></h4>

    

    - #### Oracle
    ```SQL
    SELECT EMP_NAME, DEPT_CODE, DEPT_ID, DEPT_TITLE
    FROM EMPLOYEE, DEPARTMENT
    WHERE DEPT_CODE(+) = DEPT_ID(+);
    -- ORA-01468: outer-join된 테이블은 1개만 지정할 수 있습니다.
    --> Oracle구문으로는 FULL OUTER JOIN 불가
    ```  
    </br>

## 교차 조인(CROSS JOIN)
> ### == CARTESIAN PRODUCT
> ### 조인되는 테이블의 **각 행들이 모두 매핑**된 데이터가 검색되는 방법(곱집합)
> *JOIN 구문을 잘못 작성하는 경우 CROSS JOIN의 결과가 조회*
```SQL
SELECT EMP_NAME, DEPT_TITLE
FROM EMPLOYEE
CROSS JOIN DEPARTMENT;
```
<h4><details>
    <summary>조회 결과</summary>

![CROSS OUTER JOIN Example](images/CROSS%20OUTER%20JOIN%20Example.png)
> ### 쭉 207행 까지...

</details></h4>


## 비등가 조인(NON EQUAL JOIN)