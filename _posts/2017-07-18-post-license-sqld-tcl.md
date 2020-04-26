---
title: "SQLD SQL TCL( TRANSACTION CONTROLL LANGUAGE )"
comments: true
date: 2017-07-18
categories: [License, SQLD]
tags: [TCL, TRANSACTION]

---

### 제4절 TCL ( TRANSACTION CONTROLL LANGUAGE )

 

#### 1. 트랜잭션 개요
 - 트랜잭션은 데이터베이스의 논리적 연산단위
 
 * 트랜잭션의 특성
  - 원자성 : 트랜잭션에서 정의된 연산들은 모두 성공적응로 실행되던지 아니면 전혀 실행되지 않은 상태로 남아있어야한다.
  - 일관성 : 트랜잭션이 실행되기 전의 데이터베이스 내용이 잘못 되어 있지 않다면 트랜잭션이 실행된 이후에도
       데이터베이스의 내용에 잘못이 있으면 안된다.
   - 고립성 : 트랜잭션이 실행되는 도중에 다른 트랜잭션의 영향을 받아 잘 못된 결과를 만들어서는 안된다.
   - 지속성 : 트랜잭션이 성공적으로 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 저장된다.
  *  ** 중요함 **
  
#### 2. COMMIT 
 - 입력한 자료나 수정한 자료에 대해서 또는 삭제한 자료에 대해서 전혀 문제가 없다고 판단되면, 트랜잭션은 완료할수 있다.
 - 단지 메모리 버퍼에만 영향을 받았기 때문에 데이터의 변경 이전 상태로 복구가능
 - 현재 SELECT 문장으로 결과를 확인 가능하다.
 - 다른 사용자는 현재 사용자가 수행한 명령의 결과를 볼 수 없다.
 - 변경된 행은 잠금이 설정되어서 다른 사용자가 변경할 수 없다.
 
 *COMMIT 이후의 상태 *
  - 데이터에 대한 변경 사항이 데이터베이스에 반영된다.
  - 이전 데이터는 영원히 잃어버리게 된다.
  - 모든 사용자는 결과를 볼 수 있다.
  - 관련된 행에 대한 잠금이 풀리고, 다른 사용자들이 행을 조작할 수 있게 된다.
  
 1) AUTO COMMIT
  - SQL SERVER 의 기본 방식이며, DML, DDL 을 수행할 때마다 DBMS 가 트랜잭션을 컨트롤하는 방식. 오류가 발생하면 자동으로
   ROLLBACK 을 수행.
 2) 암시적 트랜잭션 
  - ORACLE 방식
  - 트랜잭션의 시작은 ORACLE이 처리하고 트랜잭션의 끝은 사용자가 처리한다.
 3) 명시적 트랜잭션
  - 트랜잭션의 시작과 끝을 모두 사용자가 처리하는 방식
  - BEGIN TRANSACTION 으로 시작 - > COMMIT 또는 ROLLBACK 으로 트랜잭션을 종료
 
#### 3. ROLLBACK
 - 입력한 데이터나, 수정한 데이터, 삭제한 데이터에 대해서 COMMIT 이전에는 변경 사항을 취소 할수 있는 기능

 

#### 4. SAVEPOINT
 - 저장점을 정의해서 ROLLBACK 시 저장점으로 돌아갈수 있다.
 EX) SAVEPOINT [SAVEPOINT_NAME]; 
 EX) ROLLBACK TO [SAVEPOINT_NAME];
 - SQL SERVER에서는 SAVE TRANSACTION 명렁으로 대체.
 
 
 ** 정리 **
 - CREATE, ALTER, DROP, RENAME, TRUNCATE TABLE 등 DDL 문장을 실행하면 그 전후로  자동으로 COMMIT;
 - DML 문장 이후에 COMMIT 없이 DDL 문장이 실행되면 DDL 수행 전에 자동으로 COMMIT 됨.
 - 정상적으로 접속 종료시에 자동으로 COMMIT;
 - 어플리케이션이 이상 종료로 접속이 단절되면 자동으로 ROLLBACK;


