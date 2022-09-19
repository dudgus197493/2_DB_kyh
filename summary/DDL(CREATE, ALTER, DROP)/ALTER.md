# ALTER
> ### 바꾸다, 수정하다, 변조하다
- ## 테이블에서 수정할 수 있는 것
    1. 제약조건(추가/삭제)
    2. 컬럼 (추가/수정/삭제)
    3. 이름 변경(테이블명, 제약조건명, 컬럼명)
  
</br>

## 제약조건(추가/삭제)
- ### 추가
    ```SQL
    ALTER TABLE 테이블명
    ADD [CONSTRAINT 제약조건명] 제약조건 (지정할 컬럼명)
    [REFERENCES 테이블명(컬럼명)] -- 추가할 제약조건이 FK일 경우
    ```

- ### 삭제
    ```SQL
    ALTER TABLE 테이블명
    DROP CONSTRAINT 제약조건명;
    ```

- ### Sample SQL
    ```SQL
    -- DEPARTMENT_COPY의 DEPT_TITLE 컬럼에 UNIQUE 추가
    ALTER TABLE DEPARTMENT_COPY ADD CONSTRAINT DEPT_TITLE_UK UNIQUE(DEPT_TITLE);

    -- DEPARTMENT_COPY의 DEPT_TITLE 컬럼에 UNIQUE 삭제
    ALTER TABLE DEPARTMENT_COPY DROP CONSTRAINT DEPT_TITLE_UK;
    ```
  
</br>

## 컬럼 (추가/삭제)
- ### 추가
    ```SQL
    ALTER TABLE 테이블명 ADD (컬럼명 데이터타입 [DEFAULT '값']);
    ```

- ### 수정
    ```SQL
    ALTER TABLE 테이블명 MODIFY 컬럼명 데이터 타입 -- 데이터 타입 변경
    
    ALTER TABLE 테이블명 MODIFY 컬럼명 DEFAULT '값' -- DEFAULT 값 변경

    ALTER TABLE 테이블명 MODIFY 컬럼명 NOT / NOT NULL -- NULL 여부 변경
    ```

- ### 삭제
    > *테이블은 최소 1개 이상의 컬럼이 존재해야 되기 때문에 **모든 컬럼을 다 삭제할 순 없다.*** 
    ```SQL
    ALTER TABLE 테이블명 DROP(삭제할컬럼명);

    ALTER TABLE 테이블명 DROP COLUMN 삭제할컬럼명;

    ```

- ### Sample SQL
    ```SQL
    ALTER TABLE DEPARTMENT_COPY ADD (LNAME VARCHAR(30) DEFAULT '한국');
    --> 컬럼명이 LNAME, 타입이 VARCHAR2(30), DEFAULT가 '한국'인 새로운 컬럼 추가

    ALTER TABLE DEPARTMENT_COPY MODIFY DEPT_ID VARCHAR2(3);
    --> DEPT_ID의 데이터타입을 VARCHAR2(3)으로 수정

    ALTER TABLE DEPARTMENT_COPY DROP (LNAME);
    --> LNAME 컬럼 삭제
    ```
  
</br>

## 이름변경 (컬럼, 제약조건, 테이블)

- ### 컬럼
    ```SQL
    ALTER TABLE 테이블명 RENAME 변경전컬럼명 TO 변경후컬럼명;
    ```

- ### 제약조건
    ```SQL
    ALTER TABLE 테이블명 RENAME 변경전제약조건명 TO 변경후제약조건명
    ```

- ### 테이블
    ```SQL
    ALTER TABLE 테이블명 RENAME TO 변경후테이블명
    ```
  
</br>

## 테이블 삭제
```SQL
DROP TABLE 테이블명 [CASCADE CONSTRAINTS];

DROP TABLE DCOPY; -- DCOPY 테이블 삭제
```

- ### CASCADE CONSTRAINTS
    > 삭제하려는 테이블과 연결된 FK제약 조건을 모두 삭제
    ```SQL
    DROP TABLE DCOPY CASCADE CONSTRAINTS
    ```