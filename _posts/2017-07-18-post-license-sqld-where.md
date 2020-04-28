---
title: "SQLD SQL WHERE 절"
comment: true
date: 2017-07-18
categories: [License, SQLD]
tags: [WHERE, SQL]

---

### 5절 WHERE 절


#### 1.WHERE 조건절 개요 : WHERE 절은 FROM 절 다음에 위치하며, 조건식은 아래 내용으로 구성된다.
- 칼럼명
- 비교 연산자
- 문자, 숫자, 표현식
- 비교 칼럼명

#### 2. 연산자의 종류
1) 비교 연산자
- = , > , >= , < , <=


2) SQL 연산자
- BETWEEN A AND B
- IN ( LIST )
- LIKE '비교문자열'
- IS NULL

3) 논리 연산자
- AND
- OR
- NOT


4) 부정 비교 연산자
- != , ^= , <>
- NOT 칼럼명 =
- NOT 칼럼명 >


5) 부정 SQL 연산자
- NOT BETWEEN A AND B
- NOT IN ( LIST )
- IS NOT NULL
			 
#### 3. 비교 연산자

**** 연산 우선순위 ****
- 1. 괄호 안의 연산 ()
- 2. NOT 연산자
- 3. 비교연산자,SQL 비교연산자
- 4. AND
- 5. OR
************************

**** 문자 유형 비교 방법 ****
* 양쪽이 모두 CHAR 인 경우 
- 길이가 서로 다른 CHAR 형 타입이면 작은 쪽에 SPACE 를 추가하여 길이를 같게 한 후 비교
- 서로 다른 문자가 나올 때 까지 비교
- 달라진 첫 번째 문자의 값에 따라 크기를 결정
- BLANK의 수만 다르다면 서로 같은 값으로 결정
* 어느 한쪽이 VARCHAR 타입인 경우
- 서로 다른 문자가 나올 때까지 비교
- 길이가 다르다면 짧은 것이 끝날때까지만 비교 한후 길이가 긴것이 크다고 판단
- 길이가 같고 다른 것이 없다면 같다고 판단
- VARCHAR 는 NOT NULL까지 길이를 말한다.
* 상수값과 비교할 경우
- 상수 쪽을 변수 타입과 동일하게 바꾸고 비교한다.
- 변수 쪽이 CHAR 유형이라면 CHAR유형 타입을 적용
- 변수 쪽이 VARCHAR 유형이면 VARCHAR 윻여 타입을 적용
********************************





#### 4. SQL 연산자
- BETWEEN A AND B : A 와 B의 값 사이에 있으면 TRUE ( A, B 값 포함 )
- IN ( LIST ) : 리스트의 값 중에서 하나라도 있으면 TRUE
- LIKE '비교문자열' : 비교문자열과 형태가 일치하면 TRUE
- IS NULL : NULL 인 경우 TRUE

#### 5. 논리 연산자
- AND : 앞에 있는 조건과 뒤에 오는 조건이 참이 되면 결과도 참이 된다.
- OR : 앞의 조건이 참이거나 뒤의 조건이 참이 되면 결과도 참이된다.
- NOT : 뒤에 오는 조건에 반대되는 결과를 되돌려준다.

#### 6. 부정 연산자
1) 부정 논리 연산자
- != , ^= , <> , NOT 칼럼명 =
- NOT 칼럼명 > (크지 않다)


2) 부정 SQL 연산자
- NOT BETWEEN A AND B : A-B 사이에 있지 않다
- NOT IN ( LIST ) : LIST 값에 일치하지 않는다
- IS NOT NULL : NULL 값이 아니다.

7. ROWNUL , TOP 사용
1) ROWNUM
- 한 건의 행만 가져오고 싶을 때는
EX) SELECT * FROM PLAYER WHERE ROWNUM = 1;
- 두건 이상이라면
EX) SELECT * FROM PLAYER WHER ROWNUM <=2;


2) TOP ( SQL SERVER )
 EX ) SELECT TOT(1) * FROM PLAYER;


