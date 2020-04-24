---
title: "Oracle Grouping Function"
comment: true
date: 2019-07-16
categories: [DBMS, Oracle]
tags: [ROLLUP, CUBE, SQL]

---

## Oracle Grouping Function

### 1. ROLLUP Function

>ROLLUP에 지정된 Grouping Columns의 List는 Subtotal을 생성하기 위해 사용되어지며, Grouping Columns의 수를 N이라고 했을 때 N+1 Level의 Subtotal이 생성된다. 중요한 것은, ROLLUP의 인수는 계층 구조이므로 인수 순서가 바뀌면 수행 결과도 바뀌게 되므로 인수의 순서에도 주의해야 한다.
ROLLUP의 효과를 알아보기 위해 단계별로 데이터를 출력해본다.

#####  STEP 1. 일반적인 GROUP BY 절 사용

[예제] 부서명과 업무명을 기준으로 사원수와 급여 합을 집계한 일반적인 `GROUP BY` SQL 문장을 수행한다.
``` sql
SELECT DNAME, JOB,
COUNT(*) "Total Empl",
SUM(SAL) "Total Sal"
FROM EMP, DEPT
WHERE DEPT.DEPTNO = EMP.DEPTNO
GROUP BY DNAME, JOB;
```

<center>[실행결과]</center>

| DNAME | JOB | Total Empl | Total Sal|
| :---: | :---: | :---: | :---: |
|SALES|MANAGER|1|2850|
|SALES|CLERK|1|950|
|ACCOUNTING|MANAGER|1|2450|
|RESEARCH|ANALYST|2|6000|
|ACCOUNTING|CLERK|1|1300|
|SALES|SALESMAN|4|5600|
|RESEARCH|MANAGER|1|2975|
|ACCOUNTING|PRESIDENT|1|5000|
|RESEARCH|CLERK|2|1900|

##### STEP 1-2. `GROUP BY` 절 + `ORDER BY` 절 사용 
[예제] 부서명과 업무명을 기준으로 집계한 일반적인 GROUP BY SQL 문장에 ORDER BY 절을 사용함으로써 부서, 업무별로 정렬이 이루어진다.

``` sql
SELECT DNAME, JOB,
COUNT(*) "Total Empl",
SUM(SAL) "Total Sal"
FROM EMP, DEPT
WHERE DEPT.DEPTNO = EMP.DEPTNO
GROUP BY DNAME, JOB
ORDER BY DNAME, JOB;
```
<center>[실행결과]</center>



| DNAME | JOB | Total Empl | Total Sal|
| :---: | :---: | :---: | :---: |
|ACCOUNTING	|CLERK		|1			|1300           |
|ACCOUNTING	|MANAGER		|1			|2450       |
|ACCOUNTING	|PRESIDENT	|1			|5000           |
|RESEARCH	|ANALYST		|2			|6000       |
|RESEARCH	|CLERK		|2			|1900           |
|RESEARCH	|MANAGER		|1			|2975       |
|SALES		|CLERK		|1			|950            |
|SALES		|MANAGER		|1			|2850       |
|SALES		|SALESMAN	|4			|5600           |

##### STEP 2. `ROLLUP` 함수 사용
[예제] 부서명과 업무명을 기준으로 집계한 일반적인 GROUP BY SQL 문장에 ROLLUP 함수를 사용한다.


```sql
SELECT DNAME, JOB,
COUNT(*) "Total Empl",
SUM(SAL) "Total Sal"
FROM EMP, DEPT
WHERE DEPT.DEPTNO = EMP.DEPTNO
GROUP BY ROLLUP(DNAME, JOB);
```
<center>[실행결과]</center>
|DNAME 			|JOB 		|Total Empl |	Total Sal  |
| :---: | :---: | :---: | :---: |
|SALES 			|CLERK 		|1 			|950        |
|SALES 			|MANAGER 	|1 			|2850       |
|SALES 			|SALESMAN 	|4 			|5600       |
|SALES 			| 	 		|6 			|9400       |
|RESEARCH 		|ANALYST 	|2 			|6000       |
|RESEARCH 		|CLERK 		|2 			|1900       |
|RESEARCH 		|MANAGER 	|1 			|2975       |
|RESEARCH 	  	|			|5 			|10875      |
|ACCOUNTING 	|CLERK 		|1 			|1300   		|
|ACCOUNTING 	|MANAGER 	|1 			|2450   		|
|ACCOUNTING 	|PRESIDENT 	|1 			|5000   		|
|ACCOUNTING   	|			|3 			|8750   		| 
|				|			|14 		|	29025  		|

##### STEP 3. GROUPING 함수 사용

>`ROLLUP`, `CUBE`, `GROUPING SETS` 등 새로운 그룹 함수를 지원하기 위해 GROUPING 함수가 추가되었다.
ROLLUP 이나 CUBE에 의한 소계가 계산된 결과에는 GROUPING(EXRP) = 1 이 표시되고,
그 이외 결과에는 GROUPING(EXPR = 0)이 표시된다.
GROUPING 함수와 CASE/DECODE 를 이용해, 소계를 나타내는 필드에 원하는 문자열을 지정할 수 있어, 보고서 작성시 유용하게 사용할 수 있다.

[예제] ROLLUP 함수를 추가한 집계 보고서에서 집계 레코드를 구분할 수 있는 GROUPPING 함수가 추가된 SQL 문장이다.
``` sql
SELECT DNAME, GROUPING(DNAME),
JOB, GROUPING(JOB),
COUNT(*) "Total Empl",
SUM(SAL) "Total Sal"
FROM EMP, DEPT
WHERE DEPT.DEPTNO = EMP.DEPTNO
GROUP BY ROLLUP(DNAME, JOB);
```
<center>[실행결과]</center>

|DNAME		|GROUPING(DNAME)	|JOB			|GROUPING(JOB)	|Total Empl	|Total Sal|
| :---: | :---: | :---: | :---: | :---: | :---: |
|SALES		|0				|CLERK		|0				|1			|950      |
|SALES		|0				|MANAGER		|0				|1			|2850     |
|SALES		|0				|SALESMAN	|0				|4			|5600     |
|SALES		|0				| 			|1				|6			|9400     |
|RESEARCH	|0				|CLERK		|0				|2			|1900     |
|RESEARCH	|0				|ANALYST		|0				|2			|6000     |
|RESEARCH	|0				|MANAGER		|0				|1			|2975     |
|RESEARCH	|0				| 			|1				|5			|10875    |
|ACCOUNTING	|0				|CLERK		|0				|1			|1300     |
|ACCOUNTING	|0				|MANAGER		|0				|1			|2450     |
|ACCOUNTING	|0				|PRESIDENT	|0				|1			|5000     |
|ACCOUNTING	|0				| 			|1				|3			|8750     |
|			|1	 			|			|1				|14			|29025    |


##### STEP 4-2. `ROLLUP` 함수 결합 칼럼 사용

[예제] JOB과 MGR는 하나의 집합으로 간주하고, 부서별, JOB & MGR에 대한 ROLLUP 결과를 출력한다.

``` sql
SELECT DNAME, JOB, MGR, SUM(SAL) "Total Sal"
FROM EMP, DEPT
WHERE DEPT.DEPTNO = EMP.DEPTNO
GROUP BY ROLLUP(DNAME, (JOB, MGR));
```
-- JOB, MGR을 소계시 하나의 집합으로 간주하여 구분하지 않음
<center>[실행결과]</center>

|DNAME	|JOB	| Total Empl |	Total Sal|
| :---: | :---: | :---: | :---: |
|SALES				|CLERK				|7698	|950 |
|SALES				|MANAGER			|7839	|2850|
|SALES				|SALESMAN			|7698	|5600|
|SALES				| 	 				|		|9400|
|RESEARCH   |	CLERK			|7788	|1100|
|RESEARCH   |	CLERK			|7902	|800|
|RESEARCH   |	ANALYST			|7566	|6000|
|RESEARCH   |	MANAGER			|7839	|2975|
|RESEARCH   |	 	 			|		|10875|
|ACCOUNTING |	CLERK		|7782	|1300|
|ACCOUNTING |	MANAGER		|7839	|2450|
|ACCOUNTING |	PRESIDENT	| 		|5000|
|ACCOUNTING |	 			|  		|8750|
| 		 	|	 			|				|29025|

 

ROLLUP 함수 사용시 괄호로 묶은 JOB과 MGR의 경우 하나의 집합(JOB+MGR) 칼럼으로 간주하여 괄호 내 각 칼럼별 집계를 구하지 않는다.


### 2. CUBE 함수

`ROLLUP`에서는 단지 가능한 Subtotal만을 생성하였지만, CUBE는 결합 가능한 모든 값에 대하여 다차원 집계를 생성한다. `CUBE`를 사용할 경우에는 내부적으로는 `Grouping Columns` 의 순서를 바꿔어서 또 한 번의 Query를 추가 수행해야 한다. 뿐만 아니라 Grand Total은 양쪽의 Query 에서 모두 생성이 되므로 한 번의 Query에서는 제거 되어야만 하므로 ROLLUP에 비해 시스템의 연산 대상이 많다.
이처럼 `Grouping Columns` 이 가질 수 있는 모든 경우에 대하여 Subtotal을 생성해야 하는 경우에는 CUBE를 사용하는 것이 바람직하나, `ROLLUP` 에 비해 시스템에 많은 부담을 주므로 사용에 주의해야 한다.
`CUBE` 함수의 경우 표시된 인수들에 대한 계층별 집계를 구할 수 있으며, 이때 표시된 인수들 간에는 계층구조인 ROLLUP과는 달리 평등한 관계이므로 인수의 순서가 바뀌는 경우 행간에 정렬 순서는 바뀔 수 있어도 데이터 결과는 같다.
그리고 CUBE도 결과에 대한 정렬이 필요한 경우 `ORDER BY` 절에 명시적으로 정렬 칼럼이 표시가 되어야 한다.

##### STEP 5. CUBE 함수 이용

[예제] `GROUP BY ROLLUP (DNAME, JOB)` 조건에서 `GROUP BY CUBE (DNAME, JOB)` 조건으로 변경해서 수행한다.

``` sql
SELECT CASE GROUPING(DNAME) 
WHEN 1 THEN 'All Departmants' ELSE DNAME END DNAME,
CASE GROUPING(JOB)
WHEN 1 THEN 'All Jobs' ELSE JOB END JOB,
COUNT(*) "Total Empl",
SUM(SAL) "Total Sal"
FROM EMP, DEPT
WHERE DEPT.DEPTNO = EMP.DEPTNO
GROUP BY CUBE(DNAME, JOB);
```
<center>[실행결과]</center>

|DNAME		|JOB						|Total Empl	|Total Sal |
| :---:      |:---:					|:---:		|:---:     |
|All 		|Departments	All Jobs	|14			|29025     |
|All 		|Departments	CLERK		|4			|4150      |
|All 		|Departments	ANALYST		|2			|6000      |
|All 		|Departments	MANAGER		|3			|8275      |
|All 		|Departments	SALESMAN	|4			|5600      |
|All 		|Departments	PRESIDENT	|1			|5000      |
|SALES		|All Jobs				|6			|9400      |
|SALES		|CLERK					|1			|950       |
|SALES		|MANAGER					|1			|2850      |
|SALES		|SALESMAN				|4			|5600      |
|RESEARCH	|All Jobs				|5			|10875     |
|RESEARCH	|CLERK					|2			|1900      |
|RESEARCH	|ANALYST					|2			|6000      |
|RESEARCH	|MANAGER					|1			|2975      |
|ACCOUNTING	|All Jobs				|3			|8750      |
|ACCOUNTING	|CLERK					|1			|1300      |
|ACCOUNTING	|MANAGER					|1			|2450      |
|ACCOUNTING	|PRESIDENT				|1			|5000      |
 

 

CUBE는 GROUPING COLUMNS이 가질 수 있는 모든 경우의 수에 대하여 Subtotal을 생성하므로 GROUPING COLUMNS의 수가 N이라고 가정하면, 2의 N승 LEVEL의 Subtotal을 생성하게 된다.
실행 결과에서 CUBE 함수 사용으로 ROLLUP 함수의 결과에다 업무별 집계까지 추가해서 출력할 수 있는데, ROLLUP 함수에 비해 업무별 집계를 표시한 5건의 레코드가 추가된 것을 확인할 수 있다.

이처럼 `ROLLUP` 과 `CUBE` 함수를 사용하면 어려운 집계 결과를 쉽게 가져올 수 있다


