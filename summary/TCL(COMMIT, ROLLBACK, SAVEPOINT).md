# TCL(TRANSACTION CONTROL LANGUAGE)
> ### 트랜잭션 제어 언어
> ### COMMIT, ROLLBACK, SAVEPOINT

# TRANSACTION이란?
> ### 데이터베이스의 논리적 연산 단위
> ### 데이터 변경 사항을 묶어 하나의 트랜잭션에 담아 처리함
> ### 트랜잭션의 대상이 되는 데이터 변경 사항 : INSERT, UPDATE, DELETE, MERGE
```
INSERT 수행 ---------------------> DB 반영(X)

INSERT 수행 --> 트랜잭션에 추가 --> COMMIT --> DB반영(O)

INSERT 10번 수행 --> 1개 트랜잭션에 10개 추가 --> ROLLBACK --> DB 반영 안됨
```  
</br>

## COMMIT
> ### 메모리 버퍼(트랜잭션)에 임시 저장된 데이터 변경 사항을 DB에 반영
> ### 트랜잭션 종료 후 저장  
```SQL
INSERT INTO DEPARTMENT VALUES('T4', '개발4팀', 'L2');
COMMIT; -- 커밋 하기전 변경점을 DB에 반영
```

## ROLLBACK
> ### 메모리 버퍼(트랜잭션)에 임시 저장된 데이터 변경 사항을 삭제하고 **마지막 COMMIT 상태로 돌아감.**
> ### 트랜잭션 취소
```SQL
INSERT INTO DEPARTMENT VALUES('T4', '개발4팀', 'L2');
COMMIT; -- 위 변경점 DB에 반영

INSERT INTO DEPARTMENT VALUES('T5', '개발5팀', 'L2');
INSERT INTO DEPARTMENT VALUES('T6', '개발6팀', 'L2');
ROLLBACK; -- 위 변경점을 트랜잭션에서 삭제 후 마지막 COMMIT으로 이동
```
<h4><details>
    <summary>ROLLBACK 전 (IDE를 사용하여 트랜잭션안의 변경점 조회)</summary>

![ROLLBACK Example](../summary/images/TCL/ROLLBACK%20Example.png);
</details></h4> 


<h4><details>
    <summary>ROLLBACK 후</summary>

![ROLLBACK Example](../summary/images/TCL/ROLLBACK%20Example2.png);
</details></h4>   
</br>

## SAVEPOINT
> ### 메모리 버퍼(트랜잭션)에 저장 지점을 정의하여 ROLLBACK 수행 시 전체 작업을 삭제하는 것이 아닌 저장 지점까지만 일부 ROLLBACK
> ### 임시저장
```SQL
INSERT INTO DEPARTMENT VALUES('T4', '개발4팀', 'L2');
SAVEPOINT SP1;

INSERT INTO DEPARTMENT VALUES('T5', '개발5팀', 'L2');
SAVEPOINT SP2;

INSERT INTO DEPARTMENT VALUES('T6', '개발6팀', 'L2');
SAVEPOINT SP3;

ROLLBACK TO SP2; -- SP2 이후 변경점인 개발6팀 ROLLBACK

ROLLBACK TO SP1; -- SP1 이후 변경점인 개발5팀, 개발6팀 롤백

ROLLBACK TO SP3; -- ORA-01086: 'SP3' 저장점이 이 세션에 설정되지 않았거나 부적합합니다.
                 --> SP3이 저장되기전 SAVEPOINT 으로 ROLLBACK하여 SP3도 트랜잭션에서 변경점과ㄱ 같이 삭제
```

<h4><details>
    <summary>ROLLBACK 전 (IDE를 사용하여 트랜잭션안의 변경점 조회)</summary>

![ROLLBACK Example](../summary/images/TCL/ROLLBACK%20Example.png);
</details></h4> 

<h4><details>
    <summary>ROLLBACK TO SP2</summary>

![ROLLBACK Example](../summary/images/TCL/SAVEPOINT%20Example.png);
</details></h4> 

<h4><details>
    <summary>ROLLBACK TO SP1</summary>

![ROLLBACK Example](../summary/images/TCL/SAVEPOINT%20Example2.png/);
</details></h4> 

