---
title: "SQLD SQL DML( DATA MANIPULATION LANGUAGE )"
comment: true
date: 2017-07-18
categories: [License, SQLD]
tags: [DML]

---

### 제3절 DML ( DATA MANIPULATION LANGUAGE )

 

 #### 1. INSERT - 테이블에 데이터를 입력
  EX) INSERT INTO [TABLE_NAME](COL1,COL2,..) VALUES (NEW:COL1,NEW:COL2..);
  EX) INSERT INTO EMPLOYEE(EMPNO,ENAME,..) VALUES(8332,'NEWNAME',..);


 #### 2. UPDATE - 테이블에 데이터를 수정
   EX) UPDATE [TABLE_NAME] SET 수정할 칼럼명 = 새로운 값
   EX) UPDATE EMPLOYEES SET DEPT_NO = '40';


 #### 3. DELETE - 테이블의 데이터를 삭제
   EX) DELETE FROM [TABLE_NAME]; - 데이터 전부 삭제
   EX) DELETE FROM [TABLE_NAME] WHERE 조건  - 조건에 만족하는 데이터 삭제


 #### 4. SELECT - 테이블 데이터를 조회
  EX) SELECT [ALL/DISTINCT] COL1,COL2,.. FROM [TABLE_NAME]
  ** ALL : DEFAULT 옵션이므로 생략 가능
  ** DISTINCT : 중복된 데이터일 경우 1건으로 처리해서 출력



