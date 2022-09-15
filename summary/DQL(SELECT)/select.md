# SELECT 
> ### ResultSet : 데이터를 조회한 결과, SELECT구문에 의해 **조회된 행들의 집합** 을 의미.
> ### ResultSet은 **0개 이상**의 행이 포함될 수 있다.
> ### ResultSet은 특정한 기준에 의해 정렬 가능
> ### 한 테이블의 특정 컬럼, 특정 행, 특정 행/컬럼 또는 여러 테이블의 특정 행/ 컬럼 조회 가능

## 기본 
```SQL
SELECT 컬렴명 [, 컬럼명, ...]
FROM 테이블명;
```
- ### SELECT : 
   - 조회하고자 하는 컬럼명 기술  
   - 여러 컬럼을 조회하는 경우 컬름은 쉼표로 구분, 마지막 컬럼 다음은 쉼표를 사용하지 않음  
   - 모든 컬럼 조회시 컬럼 명 대신 '*' 기호 사용 가능하며 조회 결과는 기술한 컬럼 명 순으로 표시 됨

- ### FROM :
  - 조회 대상 컬럼이 포함된 테이블 명 기술
```SQL
-- 위 규칙을 지킨 샘플 SQL
-- SampleSQL 1
SELECT EMP_ID, EMP_NAME, PHONE 
FROM EMPLOYEE;
--> EMPLOYEE 테이블에서 EMP_ID, EMP_NAME, PHONE 컬럼 조회

-- SampleSQL 2
SELECT * FROM EMPLOYEE
--> EMPLOYEE 테이블에서 모든 컬럼 조회
```  
</br>

## 컬럼 값 산술 연산
> ### 컬럼 값 : 테이블 내 한칸(== 한 셀)에 작성된 값(DATA)
> ### 산술 연산은 **숫자(NUMBER)** 타입만 가능하다
```SQL
SELECT EMP_ID, EMP_NAME, SALARY, SALARY * 12 -- 연봉 (급여 * 12달)
FROM EMPLOYEE;

SELECT EMP_NAME * 10
FROM EMPLOYEE;
-- ORA-01722: 수치가 부적합합니다.
-- 산술 연산은 숫자(NUMBER)타입만 가능하다.
```  
</br>

## 날짜(DATE) 타입 조회
> ### SYSDATE : 시스템상의 현재 시간(날짜) 나타내는 함수
> ### DATE타입도 산술연산 가능
```SQL
-- 현재 시간 조회
SELECT SYSDATE
FROM DUAL; -- DUAL(DUmmy tAbLe) 테이블 : 가짜 테이블(임시 조회용 테이블)

-- 날짜 + 산술 연산(+, -)
SELECT SYSDATE - 1, SYSDATE, SYSDATE + 1
FROM DUAL;
--> 날짜의 +/- 연산 시 일 단위로 계산이 진행됨.
--> SYSDATE - 1 : 어제
--> SYSDATE + 1 : 내일
```  
</br>

## 컬럼 별칭 지정
> ### SELECT 조회 결과의 집합인 RESULT SET에 출력되는 **컬럼명을 지정**
> ### 컬럼명 AS 별칭 : 띄어쓰기X, 특수문자X, 문자만O  
> ### 컬럼명 AS "별칭" : 띄어쓰기, 특수문자O, 문자만O
> *AS는 **생략가능***
```SQL
SELECT SYSDATE-1 "하루 전", SYSDATE AS 현재시간, SYSDATE+1 내일
FROM DUAL;
```  
</br>

## 리터털
> ### 임의로 지정한 값을 SELECT절에 사용하여 **기존 테이블에 존재하는 값 처럼** 사용하는 것
> ### ' ' : DB의 기본적인 리터럴 표기법
> ### " " : 특수문자, 대소문자, 기호 등을 구분하여 나타낼 때 사용, 쌍따옴표 안에 작성되는 것들이 '하나'임을 의미
```SQL
SELECT EMP_NAME, SALARY, '원 입니다.'
FROM EMPLOYEE;
```
||EMP_NAME|SALARY|원 입니다.|
|:--:|:--:|:--:|:--|
|1|홍길동|3000000|원 입니다.|
|2|아이언맨|4000000|원 입니다.|
|3|스파이더맨|6000000|원 입니다.|
|...|

## DISTINCT 
> ### 조회 시 컬럼에 포함된 중복 값을 한 번만 표기
```SQl
SELECT DISTINCT DEPT_CODE, JOB_CODE
FROM EMPLOYEE;
```
- ### 작성 시 주의 사항
    1. `DISTINCT` 구문은 `SELECT` 마다 **딱 한번씩만 작성** 가능.
    2. `DISTINCT` 구문은 `SELECT` **제일 앞에 작성**되어야 한다.
    3. 둘 이상의 컬럼앞에 `DISTINCT` 튜플 전체 값이 동일한 경우에 한해서 중복 제거