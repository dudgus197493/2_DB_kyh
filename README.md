# SQL | DATABASE

## 할일
- [x] select.md 절 마다 파일 나누기
- [x] join.md 정리
- [x] DML.md 정리
- [X] TCL.md 정리
- [ ] SubQuery.md 정리
- [x] DDL.md 정리
- [x] DCL.md 정리
- [ ] 결과가 필요한 Sample SQL에 조회결과 이미지 넣기
- [x] README.md 파일에 SQL, DB, DBMS 요약 정리
- [x] README.md 파일에 각 챕터별 문서링크 넣기
- [ ] DML.md 파일 서브쿼리 사용구문에 서브쿼리 파일링크 걸기
- [ ] VIEW.md 파일 VIEW에서 DML사용시 주의점 예시 작성
- [ ] SEQUENCE.md 파일 SEQUENCE 사용 예시 작성


## SQL(Structured Query Language)
> ###  관계형 데이터베이스에서 원하는 데이터를 조회, 조작 하기 위해 조건을 기술하는 언어

|분류|용도|명령어|
|:--:|:--:|:--:|
|**DQL**</br>(Data Query Language)|데이터 검색| [SELECT](summary/DQL(SELECT)/head-select.md)</br>(SELECT는 DML로도 분류)|
|**DML**</br>(Data Manipulation Language)|데이터 조작|[INSERT, UPDATE, DELETE](summary/DML(INSERT,%20UPDATE,%20DELETE).md)|
|[DDL</br>(Data Definition Language)](summary/DDL(CREATE,%20ALTER,%20DROP)/head-DDL.md)|데이터 정의|[CEATE](summary/DDL(CREATE,%20ALTER,%20DROP)/CREATE.md), [DROP, ALTER](summary/DDL(CREATE,%20ALTER,%20DROP)/ALTER.md)|
|DCL</br>(Data Control Language)|데이터 제어|[GRANT, REVOKE](summary/DCL(GRANT,%20REVOKE).md)|
|**TCL**</br>(Transaction Control Language)|트랜젝션 제어|[COMMIT, ROLLBACK](summary/TCL(COMMIT,%20ROLLBACK,%20SAVEPOINT).md)|  

</br>

# Data와 Database

- ## Data
    > ### 관찰 결과로 나타난 **정량적** 혹은 정성적인 실제 값

- ## 정보
    > ### 데이터를 기반으로 의미를 부여한 것
    > *에베레스트의 높이 : 8848m -> **Data***  
    > *에베레스트는 세계에서 가장 높은 산이다. -> **정보***

- ## Database
    > #### 한 조직에 필요한 정보를 여러 응용 시스템에서 공용할 수 있도록 **논리적**으로 연관된 데이터를 모으고 **중복되는 데이터를 최소화**하여 **구조적**으로 통합/저장해놓은 것  
    </br>


## Database
- ### 정의
    1. 공용 데이터(Shared Data)
    
            공동으로 사용되는 데이터
    
    2. 통합 데이터(Integrated Data)
   
            중복 최소화로 중복으로 인한 데이터 불일치 현상 제거
    
    3. 저장 데이터(Stored Data)

            컴퓨터 저장장치에 저장된 데이터
    
    4. 운영 데이터(Operational Data)

            조직의 목적을 위해 사용되는 데이터
            
- ### 특징
    1. #### 실시간 접근성(real time accessibility)
       - 사용자가 데이터 요청 시 실시간으로 결과 서비스

    2. #### 계속적인 변화(continuos change)
       - 데이터 값은 시간에 따라 항상 바뀜

    3. #### 동시 공유(concurrent sharing)
        - 서로 다른 업무 또는 여러 사용자에게 동시 공유됨

    4. 내용에 따른 참조(reference by content)
        - 데이터의 물리적 위치가 아닌 데이터 값에 따라 참조  

</br>

## DBMS
> ### 데이터베이스에서 데이터 **추출, 조작, 정의, 제어**
>  등을 할 수 있게 해주는 데이터베이스 전용 관리 프로그램

- ### 기능
    - #### 데이터 추출(Retrieval)
        - 사용자가 조회하는 데이터 혹은 응용 프로그램의 데이터 추출

    - #### 데이터 조작(Manipulation)
        - 데이터를 조작하는 소프트웨어(응용 프로그램)가 요청하는 데이터 **삽입, 수정, 삭제, 작업** 지원

    - #### 데이터 정의(Definition)    
        - **데이터의 구조를 정의**하고 데이터 구조에 대한 삭제 및 변경 기능 수행

    - #### 데이터 제어(Control)
        - 데이터베이스 사용자를 생성하고 모니터링하여 접근 제어
        - 백업과 회복, 동시성 제어 등의 기능 지원

- ### DBMS 종류와 특징
![DBMS 종류와 특징](https://velog.velcdn.com/images/jungjuyoung/post/39a8d1db-2556-4069-832c-fa1ff45284ac/image.png)

- ### DBMS 사용 이점
    1. #### 데이터 독립화
        - 데이터와 응용 프로그램을 분리시킴으로써 상호 영향 정도를 줄일 수 있음
    2. #### 데이터 중복 최소화, 데이터 **무결성** 보장
        - 중복되는 데이터를 최소화 시키면 데이터 **무결성**이 손상될 가능성이 줄어듦
        - 중복되는 데이터를 최소화 시키면 필요한 저장공간의 낭비를 줄일 수 있음

    3. #### 데이터 보안 향상
        - 응용프로그램은 DBMS를 통해 **DBMS가 허용하는 데이터**에만 접근 가능
        - 권한에 맞게 데이터 접근을 제한하거나 데이터를 암호화시켜 저장 가능

    4. #### 관리 편의성 향상
        - 다양한 방법으로 데이터 백업 가능
        - 장애 발생 시 데이터 복구 가능  
</br>

## Database 유형
- ### 계층형 데이터베이스
    - **트리 형태**의 **계층적** 구조를 가진 데이터베이스, 최상위 계층의 데이터부터 검색하는 구조

    ![계층형 데이터베이스](https://velog.velcdn.com/images/jungjuyoung/post/23237e78-7d63-4724-9448-4a02bc7665ff/image.png)

- ### 네트워크형 데이터베이스
    - **하위 데이터들끼리의 관계까지 정의**할 수 있는 구조로 설꼐 및 구현이 복잡하고 어려움

    ![네트워크형 데이터베이스](https://velog.velcdn.com/images/jungjuyoung/post/45024ecf-2207-40b2-809c-871c68ed9e47/image.png)

- ### 관계형 데이터베이스
    - 모든 데이터를 **2차원 테이블 형태**로 표현  
    - 테이블 사이의 비즈니스적 **관계**를 도출하는 구조
    - **데이터의 중복을 최소화** 할 수 있으며 업무 변화에 대한 적응력 우수

    ![관계형 데이터베이스](https://www.oppadu.com/wp-content/uploads/2021/03/%EA%B4%80%EA%B3%84%ED%98%95-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B4%80%EA%B3%84-511x300.png)

- ###  객체-관계형 데이터베이스
    1. #### 사용자 정의 타입 지원
        - 사용자가 임의로 정한 데이터 유형으로 기본형을 넘어 다양한 형태의 데이터를 다룰 수 있음
    2. #### 참조(reference)타입 지원
        - 객체들로 이루어진 객체 테이블의 경우 하나의 레코드가 다른 레코드를 참조할 수 있는 것
    3. #### 중첩 테이블 지원
        - 테이블을 구성하는 로우(row)자체가 또 다른 테이블로 구성되는 테이블을 지원하며 조금 더 복잡하고 복합적인 정보 표현 가능
    4. #### 대단위 객체의 저장 및 추출 가능
        - 이미지, 오디오, 비디오 등 저장하기 위한 대단위 객체(LOB) 지원
    5. #### 객체간의 상속관계 지원
        - 오라클의 경우 OBJECT타입을 지원함으로써 상속 기능을 구현하고 있음