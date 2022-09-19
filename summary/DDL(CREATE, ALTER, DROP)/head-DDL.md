# DDL(Data Definition Language) : 데이터 정의 언어
> ### **객체(OBJECT)** 를 만들고(CREATE), 수정(ALTER)하고, 삭제(DROP)
> ### 데이터의 **전체 구조를 정의**하는 언어
> *주로 DB관리자 ,설계자가 사용*  

## DDL 주의 사항
- ### DDL은 COMMIT / ROLLBACK 되지 않는다.
- ### DDL과 DML 구문을 섞어서 수행하면 안된다.
  - DDL은 수행 시 **존재하고 있는 트랜잭션을 모두 DB에 강제 COMMIT**
  - **DDL이 종료된 후 DML 구문을 수행**할 수 있도록 권장
</br>

## Oracle에서의 객체
|구분|정의|
|:--:|:--|
|테이블(TABLE)||
|뷰(VIEW)||
|시퀀스(SEQUENCE)||
|인덱스(INDEX)||
|패키지(PACKAFGE)||
|트리거(TRIGGER)||
|프로시져(PROCEDURE)||
|함수(FUNCTION)||
|동의어(SYNONYM)||
|사용자(USER)||

## 데이터 딕셔너리
> ### 자원을 효율적으로 관리하기 위한 다양한 정보를 저장하는 **시스템 테이블**
> ### 사용자가 테이블을 생성하거나 사용자를 변경하는 등의 작업을 할 때 데이터베이스 **서버에 의해 자동으로 갱신되는 테이블**

