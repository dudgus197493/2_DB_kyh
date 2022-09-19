# CREATE
> ### 테이블이나 인덱스, 뷰 등 다양한 데이터베이스 객체를 생성하는 구문
> ### 테이블로 생성된 객체는 DROP 구문을 통해 제거 할 수 있음

## 테이블 생성
```SQL
CREATE TABLE 테이블명(
    컬럼명 자료형(크기),
    컬럼명 자료형(크기),
    ...
)
```
- ### Oracle의 자료형
    ![Oracle의 자료형](https://kookyungmin.github.io/image/Oracle_image/Oracle_image_18.png)

- ### Sample SQL
    ```SQL
    CREATE TABLE TB_MEMBER (
        MEMBER_ID VARCHAR2(20),
        MEMBER_PWD VARCHAR2(20),
        MEMBER_NAME VARCHAR2(30), -- UTF-8 : 영어, 숫자 1BYTE / 한글, 특수문자등 3BYTE
        MEMBER_SSN CHAR(14),
        ENROLL_DATE DATE DEFAULT SYSDATE
    );  
    ```

## 목차
### [컬럼에 주석 달기](#컬럼에-주석-달기)
### [제약조건](#제약조건)
  - [`DEFAULT`](#제약조건---default)
  - [`NOT NULL`](#제약조건---not-null)
  - [`PRIMARY KEY`](#제약조건---primary-key기본키)
  - [`UNIQUE`](#제약조건---unique)
  - [`CHECK`](#제약조건---check)
  - [`FOREIGN KEY`](#제약조건---foreign-key외부키--외래키)


</br>

## 컬럼에 주석 달기
```SQL
COMMENT ON COLUMN 테이블명.컬럼명 IS `주석내용`;
```

- ### Sample SQL
    ```SQL
    COMMENT ON COLUMN MEMBER.MEMBER_ID IS '회원 아이디';
    COMMENT ON COLUMN MEMBER.MEMBER_PWD IS '회원 비밀번호';
    COMMENT ON COLUMN MEMBER.MEMBER_NAME IS '회원 이름';
    COMMENT ON COLUMN MEMBER.MEMBER_SSN IS '회원 주민등록번호';
    COMMENT ON COLUMN MEMBER.ENROLL_DATE IS '회원 가입일';
    ```

## 제약조건(CONSTRAINTS)
> ### **사용자가 원하는 조건의 이터만 유지**하기 위해서 특정 컬럼에 설정하는 제약
> ### **데이터 무결성 보장** 을 목적으로 함
> ### 입력 데이터에 문제가 없는지 자동으로 검사하는 목적
> ### 데이터의 수정/삭제 가능여부 검사등을 목적으로 함 
> *--> 제약조건을 위배하는 DML 구문은 수행할 수 없음!*
- ### 컬럼 레벨
    - 테이블 생성 시 컬럼을 정의하는 부분에 작성
    ```SQL
    [CONSTRAINT 제약조건명] 제약조건
    ```
- ### 테이블 레벨        
    - 테이블 생성 시 컬럼 정의가 끝난 후 작성
    ```SQL
    [CONSTRAINT 제약조건명] 제약조건(컬럼명)
    ```
- ### [CONSTRAINT 제약조건명]
    - 제약조건의 이름을 지정할 수 있음
    - 생략 가능

- ### 제약조건 종류
    - `DEFAULT`
    - `NOT NULL`
    - `PRIMARY KEY`
    - `UNIQUE`
    - `CHECK`
    - `FOREIGN KEY`  
</br>

## 제약조건 - DEFAULT 
> ### 아무것도 입력하지 않아도 `NULL` 값이 아닌 설정한 값이 자동입력 되는 제약조건
```SQL
[컬럼명][타입] DEFAULT 기본값
```
- ### Sample SQL
    ```SQL
    -- DEFAULT SYSDATE
    -- 가입일 -> DEFAULT 활용(테이블 생성 시 정의된 값이 반영됨)
    INSERT INTO "MEMBER" VALUES ('MEM03', '4321REWQ', '아이언맨', '960908-9859192', DEFAULT);

    -- 가입일 -> INSERT 시 미작성 하는 경우 -> DEAFULT 값이 반영됨
    INSERT INTO "MEMBER" (MEMBER_ID, MEMBER_PWD, MEMBER_NAME) VALUES ('MEM04', 'REWQ1234', '스파이더맨');
    ```

    <h4><details>
    <summary>INSERT 아이언맨</summary>

    ![DELETE Example](../images/DDL/DEFAULT%20Example.png)
    > ### VALUES()에 DEFAULT를 작성했지만 기본값이 적용됨
    </details></h4> 

    <h4><details>
    <summary>INSERT 스파이더맨</summary>

    ![DELETE Example](../images/DDL/DEFAULT%20Example2.png)
    > ### 컬럼에 아무런 값도 작성하지 않았지만 기본값(SYSDATE)가 적용됨
    </details></h4>  
</br>

## 제약조건 - NOT NULL
> ### 해당 컬럼에 **반드시 값이 기록되어야 하는 경우** 사용
> ### 삽입/수정 시 NULL값을 허용하지 않도록 **컬럼레벨** 에서 제한
```SQL
CREATE TABLE USER_USED_NN(
    USER_NO NUMBER [CONSTRAINT USER_NO_NN] NOT NULL, 
    USER_ID VARCHAR2(20),		
    USER_PWD VARCHAR2(30),
);

INSERT INTO USER_USED_NN
VALUES(NULL, 'user01', 'pwd01'); -- ORA-01400: cannot insert NULL into ("KH"."USER_USED_NN"."USER_NO")
--> NOT NULL 제약 조건에 위배되어 오류 발생
```  
</br>

## 제약조건 - UNIQUE
> ### 컬럼에 입력 값에 대해서 **중복을 제한**하는 제약조건
> ### **컬럼레벨**, **테이블레벨** 에서 설정 가능
> *단, UNIQUE 제약 조건이 설정된 컬럼에 NULL 값은 중복 삽입 가능*
```SQL
CREATE TABLE USER_USED_UK(
    USER_NO NUMBER,
    USER_ID VARCHAR2(20) [CONSTRAINT USER_ID_UK] UNIQUE, -- 컬럼 레벨 (제약조건명 지정)
    USER_ID VARCHAR2(20),
    USER_PWD VARCHAR2(30) ,
    USER_NAME VARCHAR2(30),
   	/*테이블 레벨*/
    [CONSTRAINT USER_ID_UK] UNIQUE(USER_ID) -- 테이블 레벨(제약조건 지정)

    INSERT INTO USER_USED_UK
    VALUES(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');

    INSERT INTO USER_USED_UK
    VALUES(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');
    ); -- ORA-00001: 무결성 제약 조건(KH_KYH.USER_ID_UK)에 위배됩니다
        --> 같은 아이디인 데이터가 이미 테이블에 있으므로 UNIQUE 제약 조건에 위배되어 오류발생

    INSERT INTO USER_USED_UK
    VALUES(1, NULL, 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');
    --> 아이디에 NULL 값 삽입 가능.

    INSERT INTO USER_USED_UK
    VALUES(1, NULL, 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');
    --> 아이디에 NULL 값 중복 삽입 가능.
```
- ### UNIQUE 복합키
    > #### 두 개 이상의 컬럼을 묶어서 하나의 UNIQUE 제약조건을 설정
    > #### 복합키 지정은 **테이블 레벨만 가능**
    > #### 모든 컬럼의 값이 같을 때 위배 
    ```SQL
    CREATE TABLE USER_USED_UK2(
    USER_NO NUMBER,
    USER_ID VARCHAR2(20),
    USER_PWD VARCHAR2(30) NOT NULL,
    USER_NAME VARCHAR2(30),
    -- 테이블 레벨 UNIQUE 복합키 지정
    [CONSTRAINT USER_ID_NAME_U] UNIQUE(USER_ID, USER_NAME)

    INSERT INTO USER_USED_UK2
    VALUES(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');

    INSERT INTO USER_USED_UK2
    VALUES(2, 'user02', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');
    --> USER_NAME('홍길동')이 같지만 USER_ID('user01', 'user02')가 다르므로 제약조건 위배 X

    INSERT INTO USER_USED_UK2
    VALUES(4, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');  -- ORA-00001: 무결성 제약 조건(KH_KYH.USER_ID_NAME_U)에 위배됩니다.
    --> USER_NAME('홍길동')과 USER_ID('user01')가 모두 같으므로 제약조건 위배 O

    -- 여러 컬럼을 묶어서 UNIQUE 제약 조건이 설정되어 있으면 두 컬럼이 모두 중복되는 값일 경우에만 오류 발생 
    );
    ```

</br>

## 제약조건 - PRIMARY KEY(기본키)
> ### 테이블에서 한 행의 정보를 찾기위해 사용할 컬럼을 의미함
> ### 테이블에 대한 **식별자**(IDENTIFIER) 역할
> ### 한 테이블당 한 개만 설정 가능
> ### **컬럼레벨**, **테이블레벨**에서 설정 가능
> *NOT NULL + UNIQUE, 중복되지 않는 값이 필수로 존재*
```SQL
CREATE TABLE USER_USED_PK(
	USER_NO NUMBER [CONSTRAINT USER_NO_PK] PRIMARY KEY, -- 컬럼 레벨
    USER_ID VARCHAR2(20) UNIQUE,
    USER_PWD VARCHAR2(30) NOT NULL,
    USER_NAME VARCHAR2(30),
	[CONSTRAINT USER_NO_PK] PRIMARY KEY(USER_NO); -- 테이블 레벨

    INSERT INTO USER_USED_PK
    VALUES(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');

    INSERT INTO USER_USED_PK
    VALUES(1, 'user02', 'pass02', '이순신', '남', '010-5678-9012', 'lee123@kh.or.kr');
    --> 기본키 중복으로 오류

    INSERT INTO USER_USED_PK
    VALUES(NULL, 'user03', 'pass03', '유관순', '여', '010-9999-3131', 'yoo123@kh.or.kr');
    --> 기본키가 NULL 이므로 오류
);
```

- ### PRIMARY KEY 복합키
    > #### 두 개 이상의 컬럼을 묶어서 하나의 PRIMARY KEY 제약조건을 설정
    > #### 복합키 지정은 **테이블 레벨만 가능**
    > #### 모든 컬럼의 값이 같을 때 위배 
    > #### 모든 컬럼중 하나의 컬럼이라도 NULL일 때 위배
    ```SQL
    CREATE TABLE USER_USED_PK2(
    USER_NO NUMBER,
    USER_ID VARCHAR2(20),
    USER_PWD VARCHAR2(30) NOT NULL,
    USER_NAME VARCHAR2(30),
    [CONSTRAINT PK_USERNO_USERID] PRIMARY KEY(USER_NO, USER_ID) -- 복합키

    INSERT INTO USER_USED_PK2
    VALUES(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@kh.or.kr');

    INSERT INTO USER_USED_PK2
    VALUES(1, 'user02', 'pass02', '이순신', '남', '010-5678-9012', 'lee123@kh.or.kr');

    INSERT INTO USER_USED_PK2
    VALUES(1, 'user01', 'pass01', '신사임당', '여', '010-9999-9999', 'sin123@kh.or.kr'); -- ORA-00001: 무결성 제약 조건(KH_KYH.PK_USERNO_USERID)에 위배됩니다
    --> USER_NO(1)와 USER_ID('user01')가 중복이므로 제약조건 위배 O

    INSERT INTO USER_USED_PK2
    VALUES(NULL, 'user01', 'pass01', '신사임당', '여', '010-9999-9999', 'sin123@kh.or.kr'); -- ORA-01400: NULL을 ("KH_KYH"."USER_USED_PK2"."USER_NO") 안에 삽입할 수 없습니다
    --> USER_NO, USER_ID 둘 중 하나라도 NULL이면 제약조건 위배 O
    );
    ```  
</br>

## 제약조건 - FOREIGN KEY(외부키 / 외래키)
> ### 참조(REFERENCES)된 다른 테이블의 컬럼이 제공하는 값만 사용 가능
> ### FOREIGN KEY 제약조건에 의해서 테이블간의 관계(RELATIONSHIP)가 형성됨
> `NULL` 사용 가능
```SQL
-- 컬럼레벨
컬럼명 자료형(크기) [CONSTRAINT 이름] REFERENCES 참조할 테이블먕 [(참조할컬럼)] [삭제룰]

-- 테이블레벨
[CONSTRAINT 이름] FOREIGN KEY (적용할컬럼명) REFERENCES 참조할테이블명 [(참조할컬럼)] [삭제룰]
```
> ### PRIMARY KEY, UNUQUE 컬럼만 외래키로 사용 가능
> *참조할 컬럼명이 생략이 되면 PRIMARY KEY로 설정된 컬럼이참조 컬럼으로 설정*
```SQL
-- Sample Table
CREATE TABLE USER_GRADE(
  GRADE_CODE NUMBER PRIMARY KEY,
  GRADE_NAME VARCHAR2(30) NOT NULL
);
INSERT INTO USER_GRADE VALUES (10, '일반회원');
INSERT INTO USER_GRADE VALUES (20, '우수회원');
INSERT INTO USER_GRADE VALUES (30, '특별회원');

CREATE TABLE T_USER_USED_FK(
  USER_NO NUMBER PRIMARY KEY,
  USER_ID VARCHAR2(20) UNIQUE,
  USER_PWD VARCHAR2(30) NOT NULL,
  USER_NAME VARCHAR2(30),
  GRADE_CODE NUMBER CONSTRAINT GRADE_CODE_FK REFERENCES USER_GRADE [(GRADE_CODE)] -- 컬럼 레벨
  
  CONSTRAINT GRADE_CODE_FK FOREIGN KEY (GRADE_CODE) REFERENCES USER_GRADE (GRADE_CODE) -- 테이블 레벨					
);
```
<h4><details>
    <summary>SampleTable 관계도</summary>

![DELETE Example](../images/DDL/FOREIGN%20KEY%20Example.png)
> ### 부모 테이블(USER_GRADE)과 자식 테이블(T_USER_USED_FK)의 연결
</details></h4> 

- ### Sample SQL
    ```SQL
    INSERT INTO T_USER_USED_FK
    VALUES(2, 'user02', 'pass02', '이순신', 10); -- 부모 테이블에 10 존재

    INSERT INTO T_USER_USED_FK
    VALUES(3, 'user03', 'pass03', '유관순', 30); -- 부모 테이블에 30 존재

    INSERT INTO T_USER_USED_FK
    VALUES(4, 'user04', 'pass04', '안중근', null);
    --> NULL 사용 가능.

    INSERT INTO T_USER_USED_FK
    VALUES(5, 'user05', 'pass05', '윤봉길', 50);
    -- ORA-02291: 무결성 제약조건(KH_KYH.GRADE_CODE_FK)이 위배되었습니다- 부모 키가 없습니다
    --> 50이라는 값은 USER_GRADE 테이블 GRADE_CODE 컬럼에서 제공하는 값이 아니므로 외래키 제약 조건에 위배되어 오류 발생.
    ```
- ### FOREIGN KEY 삭제 옵션
    > #### 부모 테이블의 데이터 삭제 시 자식 테이블의 데이터를 어떤식으로 처리할 지 설정

    - #### ON DELETE RESTRICTED (삭제 제한)
        > FOREIGN KEY로 지정된 컬럼에서 사용되고 있는 값일 경우 제공하는 컬럼의 값을 삭제하지 못함
        > *기본값*
        ```SQL
        DELETE FROM USER_GRADE WHERE GRADE_CODE = 30;
        -- ORA-02292: 무결성 제약조건(KH_KYH.GRADE_CODE_FK)이 위배되었습니다- 자식 레코드가 발견되었습니다

        DELETE FROM USER_GRADE WHERE GRADE_CODE = 20;
        -- GRADE_CODE 중 20은 외래키로 참조되고 있지 않으므로 삭제가 가능함.
        ```
    
    - #### ON DELETE SET NULL
        > 부모키 삭제시 자식키를 NULL로 변경하는 옵션
        ```SQL
        DELETE FROM USER_GRADE2 WHERE GRADE_CODE = 10;
        ```
        <h4><details>
        <summary>DELETE SQL 수행</summary>

        ![DELETE Example](../images/DDL/FOREIGN%20KEY%20Example2.png)
        > ### 자식테이블(T_USER_USED_FK)에 GRADE_CODE가 10이었던 컬럼이 NULL로 변경
        </details></h4> 
    
    - #### ON DELETE CASCADE
        > 부모키 삭제시 자식키도 함께 삭제
        > *부모키 삭제시 값을 사용하는 자식 테이블의 컬럼에 해당하는 행 삭제*
        ```SQL
        DELETE FROM USER_GRADE2 WHERE GRADE_CODE = 10;
        ```

        <h4><details>
        <summary>DELETE SQL 수행</summary>

        ![DELETE Example](../images/DDL/FOREIGN%20KEY%20Example3.png)
        > ### 자식테이블(T_USER_USED_FK)에 GRADE_CODE가 10이었던 컬럼이 삭제
        </details></h4>
  
</br>

## 제약조건 - CHECK
> ### 컬럼에 기록되는 값에 조건 설정을 할 수 있음
> ### 범위로도 조건 설정 가능
> ### FK의 하위호환 : 둘다 컬럼의 값을 제한하지만 CHECK는 작성 후 수정이 번거롭고, FK는 부모테이블의 행을 추가,수정,삭제가 간편함  
> *비교값은 리터럴만 사용 가능 (변하는 값, 함수 사용 X)*
```SQL
-- 작성법
CHECK (컬럼명 비교연산자 비교값)

-- Sample Table
CREATE TABLE USER_USED_CHECK(
  USER_NO NUMBER PRIMARY KEY,
  USER_ID VARCHAR2(20) UNIQUE,
  USER_PWD VARCHAR2(30) NOT NULL,
  USER_NAME VARCHAR2(30),
  GENDER VARCHAR2(10) CONSTRAINT GENDER_CHECK CHECK(GENDER IN('남', '여'))
);

INSERT INTO USER_USED_CHECK
VALUES(2, 'user02', 'pass02', '홍길동', '남자'); -- ORA-02290: 체크 제약조건(KH_KYH.GENDER_CHECK)이 위배되었습니다
-- GENDER 컬럼에 CHECK 제약조건으로 '남' 또는 '여'만 기록 가능한데 '남자'라는 조건 이외의 값이 들어와 에러 발생
```



