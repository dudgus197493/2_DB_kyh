# SEQUENCE (순서, 연속, 수열)
> ### 순차적 번호 자동 발생기 역할의 객체
> ### 객체 생성 후 호출 시 지정된 범위 내 일정한 간격으로 증가하는 숫자 출력
> ### **주로 PK역할의 컬럼에 삽입되는 값을 만드는 용도 (인위적 주식별자)**
```SQL
CREATE SEQUENCE 시퀀스명
[START WITH 숫자]
[INCREMENT BY 숫자]
[MAXVALUE 숫자 | NOMAXVALUE]
[MINVALUE 숫자 | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE | NOCACHE]
```
  
</br>

## SEQUENCE 옵션
|**구분**|**기능**|
|:--:|:--|
|START WITH 숫자|처음 발생시킬 시작값 지정, 생략하면 1이 기본|
|INCREMENT BY 숫자|다음 값에 대한 증가치, 생략하면 1이 기본|
|MAXVALUE 숫자 &#124; NOMAXVALUE|발생시킬 최대값 지정 (10의 27승 -1)|
|MINVALUE 숫자 &#124; NOMINVALUE|최소값 지정(-10의 26승)|
|CYCLE &#124; NOCYCLE|값 순환 여부 지정|
|CACHE &#124; NOCACHE|캐쉬메모리 기본값 20byte, 최소값 2byte|
- ### SEQUENCE 캐시
    > 할당된 크기만큼 미리 다음 값들을 생성해 저장
    > 매번 시퀀스 생성해서 반환하는 것보다 DB속도 향상
    > *DB 재부팅시 캐시 다음값으로 반환*
  
</br>

## SEQUENCE 사용 방법
- ### 시퀀스명.NEXTVAL
    > 다음 시퀀스 번호를 얻어옴. (INCREMENT BY 만큼 증가된 값)  
    > *단, 시퀀스 생성 후 **첫 호출**인 경우 START WITH의 값을 얻어옴*
- ### 시퀀스명.CURRVAL
    > 현재 시퀀스 번호 얻어옴  
    > 마지막 NEXTVAL 호출 값을 다시 가져옴  
    > ***단, 시퀀스 생성 후 NEXTVAL 호출 없이 CURRVAL를 호출 시 오류 발생***
- ### Sample SQL
    ```SQL
    CREATE SEQUENCE SEQ_TEST;

    SELECT SQL_TEST.CURRVAL FROM DUAL; -- ORA-08002: 시퀀스 SEQ_TEST.CURRVAL은 이 세션에서는 정의 되어 있지 않습니다

    SELECT SEQ_TEST.NEXTVAL FROM DUAL; -- 1

    SELECT SEQ_TEST.CURRVAL FROM DUAL; -- 1

    SELECT SEQ_TEST.NEXTVAL FROM DUAL; -- 2
    SELECT SEQ_TEST.NEXTVAL FROM DUAL; -- 3
    SELECT SEQ_TEST.NEXTVAL FROM DUAL; -- 4
    SELECT SEQ_TEST.NEXTVAL FROM DUAL; -- 5

    SELECT SEQ_TEST.CURRVAL FROM DUAL; -- 5
    ```
  
</br>

## SEQUENCE 수정
```SQL
ALTER SEQUENCE 시퀀스이름
  ＊＊START WITH 옵션 수정 불가＊＊
  [INCREMENT BY 숫자]
  [MAXVALUE 숫자 | NOMAXVALUE]
  [MINVALUE 숫자 | NOMINVALUE]
  [CYCLE | NOCYCLE]
  [CACHE 바이트크기 | NOCACHE]
```
