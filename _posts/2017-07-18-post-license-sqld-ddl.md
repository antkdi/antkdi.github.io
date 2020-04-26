---
title: "SQLD SQL DDL( DATA DEFENITION LANGUAGE )"
comment: true
date: 2017-07-18
categories: [License, SQLD]
tags: [DDL]

---

### 제2절 DDL ( DATA DEFENITION LANGUAGE )

 

#### 1.데이터 유형
- CHARACTER(s)
	- 고정 길이 문자열 정보
	- s 는 기본길이 1바이트, 최대길이 Oracle 2000 바이트 , SQL SERVER 8000바이트
	- s 만큼 최대 길이를 갖고 고정 길이를 가지고 있으므로 할당된 변수 값의 길이가 s 보다 작을 경우에는
	그 차이 길이 만큼 공간(SPACE) 로 채워진다.


- VARCHAR(s)
	- CHARACTER VARYING 의 약자로 가변 길이 문자열 정보 ( ORACLE 은 VARCHAR2, SQLSERVER는 VARCHAR )
	- s 는 최소 길이 1바이트 최대길이 Oracle 4000바이트, SQLSERVER 8000바이트
	- s 만큼의 최대 길이를 갖지만 가변 길이로 조정 되기 때문에 할당된 변수값의 바이트만 적용된다


- NUMERIC
	- 정수 , 실수 등 숫자 정보 ( ORACLE NUMBER, SQLSERVER 는 10개 이상의 숫자 타입을 가지고 있다 )
	- Oracle 은 처음에 전체 자리수를 지정하고, 그 다음 소수 부분의 자리수를 지정한다
	ex) NUMBER(8,2) 정수 8자리, 실수 2 자리


- DATE
	- 날짜와 시각 정보
	- Oracle 은 1초단위 SQLSERVER는 3.33ms (단위 관리

** 유의 할 내용 **
CHAR 유형의 경우 지정한 바이트 만큼 스페이스로 채우기 때문에
'AA' = 'AA   ' 가 된다.
VARCHAR 는 가변길이이기 때문에 
'AA' != 'AA  ' 
*******************

#### 2. 제약조건
- PRIMARY KEY : 테이블에 저장된 행 데이터를 고유하게 식별하기 위한 기본 키를 정의한다.
	  하나의 테이블에 하나의 기본키 제약만 정의할 수 있다. 기본키 제약을 정의하면 DBMS 는 자동으로 UNIQUE
	  INDEX를 생성하며, 기본키를 구성하는 칼럼에는 NULL값은 허용되지 않는다.
	  결국 기본키제약 -> 고유키 제약 -> NOT NULL 제약이 된다.
	  
- UNIQUE KEY : 테이블에 저장된 행 데이터를 고유하게 식별하기 위한 기본키를 정의한다. 단, NULL 은 고유키 제약 대상
	  이 아니므로 NULL 값을 가진 행이 여러개가 있더라도 고유키 제약위반이 되지 않는다.

- NOT NULL : NULL 값의 입력을 허용하지 않는다. 디폴트 상태에서는 모든 칼럼에서 NULL 을 허용하지만 , 이 제약조건
	 을 지정함으로써 해당 칼럼은 입력 필수가 된다. NOT NULL 을 CHECK 의 일부분 으로 이해할 수 있다.

- CHECK : 입력할 수 있는 값의 범위등을 제한 . CHECK 제약으로는 TRUE OR FALSE 로 평가 할수 있는 논리식을 지정

- FOREIGN KEY : 관계형 데이터베이스 에서 테이블 간의 관계를 정의하기 위해 기본키를 다른 테이블의 외래키로 복사하는
	  경우 외래키가 생성. , 외래키 지정시 참조 무결성 제약 옵션을 선택할수 있다.
	  
**** 참고 사항 ****
* NULL 의 의미 : NULL은 ASCII CODE 00번 으로 공백이나 숫자 0 과는 전혀 다른 값이고, 조건에 맞는 데이터가 없을때의
	  공집합과도 다르다. NULL 은 ' 아직 정의되지 않은 미지의 값' 이거나 ' 현재 데이터를 입력하지 못하는 경우'
*******************

#### 3. ALTER TABLE
```sql
1) ADD COLUMN - 기존 테이블의 칼럼 추가
EX) ALTER TABLE [TALBE_NAME] ADD [ADD_COLUMN_NAME] [DATA_TYPE];
EX) ALTER TABLE EMPLOYEES ADD DEPTNAME VARCHAR(20);


2) DROP COLUMN - 기존 테이블의 칼럼 삭제
EX) ALTER TABLE [TABLE_NAME] DROP COLUMN [DELETE_COLUMN_NAME];
EX) ALTER TABLE EMPLOYEES DROP COLUMN DEPTNAME;


3)MODIFY COLUMN - 기존 테이블의 칼럼 수정
EX) ALTER TABLE [TABLE_NAME] MODIFY ([COLUMN_NAME] [DATA_TYPE] [DEFAULT VALUE] [NOT NULL],...);
EX) ALTER TABLE EMPLOYEES MODIFY (ENAME  NUMBER DEFAULT 3 NOT NULL);

4)RENAME COLUMN
EX) ALTER TABLE [TABLE_NAME] RENAME COLUMN [BEFORE_COLUMN_NAME] TO [AFTER_COLUMN_NAME];
EX) ALTER TABLE EMPLOYEES RENAME COLUMN ENAME TO EMPNAME;


5) DROP CONSTRAINT - 기존 테이블 제약조건 삭제
EX) ALTER TABLE DROP CONSTRAINT [CONSTRAINT_NAME];
EX) ALTER TABLE DROP EMPLOYEES [UNIQUE_CONSTRAINT_1];


6) ADD CONSTRAINT - 기존 테이블 제약조건 추가
EX) ALTER TABLE [TABLE_NAME] ADD CONSTRAINT [NEW_CONSTRAINT_NAME] [CONSTRAINT]([COLOUMN_NAME]);
EX) ALTER TABLE EMPLOYEES ADD CONSTRAINT NULL_CONSTRAINT_1 NOT NULL(ENAME);


7) RENAME TABLE - 기존 테이블 이름 변경
EX) RENAME [BEFORE_TABLE_NAME] TO [AFTER_TABLE_NAME];


8) DROP TABLE - 테이블 삭제
EX) DROP TABLE [TABLE_NAME] [CASCADE CONSTRAINT];


** CASCADE CONSTRAINT 는 참조되는 제약 조건 까지도 모두 삭제하는 것을 의미 **
```
#### 4. TRUNCATE TABLE

- 테이블의 구조는 남기고 데이터를 모두 삭제
EX) TRUNCATE TABLE [TABLE_NAME];


