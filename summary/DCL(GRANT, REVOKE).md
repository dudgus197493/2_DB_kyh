# DCL(Data Control Language)
> ### 데이터를 다루기 위한 권한을 다루는 언어
> ### 계정에 DB, DB객체애 대한 접근 권한을 **부여(GRANT)** 하고 **회수(REVOKE)**하는 언어

## 권한의 종류
- ### 시스템권한
    > DB접속, 객체 생성 권한
    
    |구분|설명|
    |:--:|:--|
    |CRETAE SESSION|데이터베이스 접속 권한|
    |CREATE TABLE|테이블 생성 권한|
    |CREATE VIEW|뷰 생성 권한|
    |CREATE SEQUENCE|시퀀스 생성 권한|
    |CREATE PROCEDURE|함수(프로시져) 생성 권한|
    |CREATE USER|사용자(계정) 생성 권한|
    |DROP USER|사용자(계정) 삭제 권한|
    |DROP ANY TABLE|임의 테이블 삭제 권한|

- ### 객체 권한
    > 특정 객체를 조작할 수 있는 권한  

    |권한 종류|설정 객체|
    |:--:|:--|
    |SELECT|TABLE, VIEW, SEQUENCE|
    |INSERT|TABLE, VIEW|
    |UPDATE|TABLE, VIEW|
    |DELETE|TABLE, VIEW|
    |ALTER|TABLE, SEQUENCE|
    |REFERENCES|TABLE|
    |INDEX|TABLE|
    |EXECUTE|PROCEDURE|
  
</br>

## USER - 계정(사용자)
- ### 관리자 계정
    > 데이터베이스의 **생성과 관리를 담당**하는 계정.  
    > **모든 권한과 책임을 가지는** 계정
    > *sys(최고관리자), system(sys에서 권한 몇개 제외된 관리자)*

- ### 사용자 계정
    > 데이터베이스에 대하여 **질의, 갱신, 보고서 작성 등의 작업을 수행**할 수 있는 계정  
    > 업무에 필요한 **최소한의 권한만을 가지는 것을 원칙**으로 함
  
</br>

## 계정 생성과 권한 부여 - GRANT, REVOKE
```SQL
-- 계정 생성
CREATE USER 사용자명 IDENTIFIED BY 비밀번호;

-- 권한 부여
GRANT 권한 [, 권한 ....] TO 사용자명;

-- 권한 회수
REVOKE 권한 [,권한 ...] ON 객체명 FROM 계정명
```

- ### Sample SQL
    ```SQL
    -- (sys) 관리자 계정 접속
    ALTER SESSION SET "_ORACLE_SCRIPT" = TRUE; -- 예전 SQL 방식 허용

    CREATE USER kh_kyh IDENTIFIED BY kh123456; -- 계정명이 kh_kyh, 비밀번호가 kh123456인 계정 생성

    GRANT CREATE SESSION TO kh_kyh; -- kh_kyh 계정에 접속 권한 부여
    
    GRANT CREATE TABLE TO kh_kyh -- kh_kyh 계정에 테이블 생성 권한 부여

    ALTER USER kh_kyh DEFAULT TABLESPACE
    SYSTEM QUOTA UNLIMITED ON SYSTEM; -- kh_kyh 계정에 데이터를 저장할 수 있는 공간 할당

    REVOKE INSERT ON kh.EMPLOYEE FROM kh_kyh; -- kh_kyh으로부터 kh.EMPLOYEE 테이블에 INSERT 권한 회수
    ```
  
</br>

## ROLE
> ### 역할, 여러개의 권한을 그룹으로 묶은 권한의 묶음
> ### 사용자에게 ROLE 부여시 ROLE에 속한 권한들을 한꺼번에 부여
```SQL
GRANT CONNECT, RESOURCE TO kh_kyh; -- kh_kyh 계정에 CONNECT, RESOURCE ROLE 부여
-- CONNECT  : DB 접속 관련 권한을 묶어둔 ROLE
-- RESOURCE : DB 사용을 위한 기본 객체 생성 권한을 묶어둔 ROLE
```