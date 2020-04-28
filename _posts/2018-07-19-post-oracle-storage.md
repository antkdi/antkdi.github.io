---
title: "Oracle 용량 산정하기"
comment: true
date: 2018-07-19
categories: [DBMS, Oracle]
tags: [Environment, Storage, SQL]

---

## Oracle 용량 산정하기

데이터베이스의 용량을 구해야 하는일이 생겨서 찾아보았다.



### 오라클 DB 사이즈 확인

```SQL 
-- 현재 사용량
select sum(bytes)/1024/1024/1024 GB from dba_segments;  
-- 오라클이 잡고 있는 용량
select sum(bytes)/1024/1024/1024 GB from dba_data_files;
```



### 테이블별 용량 확인

``` sql 
SELECT 
	table_name, 
	num_rows * avg_row_len / 1024 / 1024  AS mb
FROM user_tables;
```



### 테이블 스페이스 용량 확인

``` sql
-- 테이블 스페이스 확인
SELECT TABLESPACE_NAME, STATUS, CONTENTS FROM DBA_TABLESPACES;

-- 테이블스페이스 파일 목록
SELECT FILE_NAME, BYTES, STATUS FROM DBA_DATA_FILES;

-- 테이블스페이스 잔여 공간
SELECT TABLESPACE_NAME, BYTES, BLOCKS FROM DBA_FREE_SPACE;
 
-- 테이블스페이스 확인
SELECT TABLESPACE_NAME, FILE_NAME, BYTES FROM dba_data_files;

-- 사용자 확인
SELECT USERNAME, DEFAULT_TABLESPACE FROM dba_users
```



### 사용자별 용량 확인

```sql
SELECT 
	SEGMENT_TYPE, 
	SEGMENT_NAME, 
	TABLESPACE_NAME, 
	TRUNC((SUM(BYTES)/1024)/1024,2) as "사이즈(MB)" 
FROM DBA_SEGMENTS 
WHERE SEGMENT_TYPE IN ('TABLE', 'INDEX', 'TABLE PARTITION', 'TABLE SUBPARTITION') 
AND OWNER = '사용자명' 
GROUP BY SEGMENT_TYPE, SEGMENT_NAME, TABLESPACE_NAME 
ORDER BY 1 desc, "사이즈(MB)" desc;

```

``` sql
SELECT 
	owner, 
	table_name, 
	TRUNC(sum(bytes)/1024/1024/1024) GB 
FROM 
	(
     SELECT 
     	segment_name, 
     	table_name, 
     	owner, 
     	bytes 
     FROM 
     	dba_segments 
     WHERE segment_type in ('TABLE','TABLE PARTITION') 
     UNION ALL 
     SELECT 
     	i.table_name, 
     	i.owner, 
     	s.bytes 
     FROM dba_indexes i, dba_segments s 
     WHERE s.segment_name = i.index_name 
     AND s.owner = i.owner 
     AND s.segment_type in ('INDEX','INDEX PARTITION') 
     UNION ALL 
     SELECT 
     	l.table_name, 
     	l.owner, 
     	s.bytes 
     FROM dba_lobs l, dba_segments s 
     WHERE s.segment_name = l.segment_name 
     AND s.owner = l.owner 
     AND s.segment_type IN ('LOBSEGMENT','LOB PARTITION') 
     UNION ALL 
     SELECT 
     	l.table_name, 
     	l.owner, 
     	s.bytes 
     FROM dba_lobs l, dba_segments s 
     WHERE s.segment_name = l.index_name 
     AND s.owner = l.owner 
     AND s.segment_type = 'LOBINDEX'
    ) 
GROUP BY table_name, owner 
ORDER BY SUM(bytes) desc


```

