---
title: "SQLD SQL 문장들의 종류"
date: 2017-07-18
comments: true
categories: [License, SQLD]
tags: [DML, DDL, DCL, TCL]

---

### SQL 문장들의 종류


#### 1. SQL
- 데이터 조작어 ( DML )
- SELECT : 데이터베이스에 들어 있는 데이터를 조회하거나 검색하기 위한 명령어 Retrieve 라고도 함.
- INSERT, UPDATE, DELTE : 테이블에 들어있는 데이터에 변형을 가하는 종류의 명령어
- 데이터 정의어 ( DDL )
- CREATE, ALTER, DROP, RENAME : 테이블과 같은 데이터 구조를 정의하는데 사용되는 명령어들로 구조를
				생성하거나 변경, 삭제, 또는 이름을 바꾸는 명령어
- 데이터 제어어 ( DCL )
- GRANT , REVOKE : 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어를 DCL이라고 함
- 트랜잭션 제어어 ( TCL )
- COMMIT, ROLLBACK : 논리적인 작업의 단위를 묶어서 DML 에 의해 조작된 결과를 작업단위 별로 제어하는 명령어     


#### 2. TABLE
- 테이블 : 행과 칼럼의 2차원 구조를 가진 데이터의 저장 장소이며 데이터베이스의 가장 기본적인 개념
- 칼럼/열 : 2차원 구조를 가진 테이블에서 세로 방향으로 이루어진 하나하나의 특정 속성
- 행 : 2차원 구조를 가진 테이블에서 가로 방향으로 이루어진 연결된 데이터



** 테이블 관계 용어 **
* 정규화 : 테이블을 분할 하여 데이터의 정합성을 확보하고, 불필요한 중복을 줄이는 프로세스
* 기본키 : 테이블에 존재하는 각 행을 한가지 의미로 특정할 수 있는 한개 이상의 칼럼
* 외부키 : 다른 테이블의 기본키로 사용되고 있는 관계를 연결하는 칼럼

3) 표기법 (예시)

- 바커 표기법   
![바커 표기법](https://t1.daumcdn.net/cfile/tistory/271D873555C029191B)  
- IE 표기법
![IE 표기법](https://t1.daumcdn.net/cfile/tistory/211F393555C0291A1C)
