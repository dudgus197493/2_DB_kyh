# INDEX(색인)
> ### SQL 명령문 중 SELECT의 처리 속도를 향상 시키기 위해 컬럼에 대해 생성하는 객체
> *인덱스 내부 구조는 B &#42; 트리 형식으로 되어있음.*

## 인덱스 장점
- #### 이진 트리 형식으로 구성되어 있어 자동 정렬 및 검색 속도가 빠름
- #### 조회 시 전체 테이블 내용을 조회하는 것이 아닌 인덱스가 지정된 컬럼만을 이용해 조회 -> **시스템 부하 낮아지고 성능 향상**
  
</br>

## 인덱스 단점
- #### 데이터 변경(INSERT, UPDATE, DELETE) 작업 빈번한 경우 성능 저하
- #### 인덱스도 하나의 객체, 이름 저장하기 위한 별도 공간 필요
- #### 인덱스 생성 시간 필요
  
</br>

## 인덱스 생성 방법
```SQL
CREATE [UNIQUE] INDEX 인덱스명
ON 테이블명(컬럼명, 컬럼명, ... | 함수명, 함수계산식);
```
- ### 인덱스 자동 생성
    - `PK` 또는 `UNIQUE` 제약조건이 설정되는 경우
  
</br>

## 인덱스를 활용한 검색 방법
> `WHERE` 절에 인덱스가 지정된 컬럼을 언급
```SQL
-- EMP_ID는 PK
SELECT * FROM EMPLOYEE
WHERE EMP_ID = 215;
```
  
</br>

## 인덱스 사용 성능 비교
```SQL
-- Sample Table
CREATE TABLE TB_IDX_TEST(
    TEST_NO NUMBER PRIMARY KEY, -- 자동으로 인덱스가 생성됨.
    TEST_ID VARCHAR2(20) NOT NULL
);
```

- ### 인덱스 사용 X
    ```SQL
    SELECT * FROM TB_IDX_TEST
    WHERE TEST_ID = 'TEST500000';
    ```
    ![INDEX Example](../images/Object/INDEX%20Example.png)

- ### 인덱스 사용 O
    ```SQL
    SELECT * FROM TB_IDX_TEST
    FROM TEST_NO = 500000;
    ```
    ![INDEX Example](../images/Object/INDEX%20Example2.png)
