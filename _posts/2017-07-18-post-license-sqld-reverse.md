---
title: "SQLD 반정규화 (Reverse Data Modeling) 요약"
date: 2017-07-18
comments: true
categories: [License, SQLD]
tags: [Reverse Data Modeling]

---

### 제3절 반정규화와 성능



#### 1. 반정규화를 통한 성능향상 전략



- 반정규화의 정의

	- 정규화된 엔터티, 속성, 관계를 시스템의 성능향상 및 개발과 운영의 

	  단순화를 위해 중복, 통합, 분리 등을 수행하는 데이터 모델링 기법

	- 디스크 I/O, 테이블끼리의 경로가 멀거나, 컬럼 계산시 조회 성능이 저하될 것이 예상될때 반정규화를 수행

	- 업무적으로 조회에 대한 처리성능이 중요할때 부분적으로 반정규화를 수행

	- 설계단계에서 적용하며, 미 수행시에는 

		- 성능이 저하된 데이터베이스가 생성.

		- 구축단계나 시험단계에 보다 반정규화를 적용시 노력, 비용이 많이 든다.



- 반정규화 적용방법

	- 컬럼 반정규화, 테이블 반정규화, 관계의 반정규화 종합적으로 고려하여 실시

	- 성능을 향상 시킬수 있는 다른 방법을 먼저 고려하고 그 이후 반정규화를 수행.



- 반정규화의 대상을 조사

	- 자주 사용되는 테이블에 엑세스하는 프로세스의 수가 많고 항상 일정한 범위만을 조회하는경우.

	- 테이블에 대량 데이터가 있고 대량의 범위를 자주 처리하는 경우.

	- 통계성 프로세스에 의해 통계정보를 필요로할 때 별도의 통계테이블 생성.

	-테이블에 지나치게 많은 조인이 되어 조회가 기술적으로 어렵거나 성능이 저하 될 경우.

  

- 반정규화의 대상에 대해서 다른 방법으로 처리가능한지 검토

	- 많은 조인을 하여 기술적으로 조회가 어려운경우 View 생성 ( 쿼리의 단순화 및 SQL 작성의 미숙함으로 인해 생기는 성능저하 방지)

	- 대량 데이터 처리나 부분처리에 의해 성능저하되는 경우 클러스터링, 인덱스조정 등을 통해 성능 향상 도모

	- PK에 성격에 따라 파티셔닝 기법을 적용

	- 어플리케이션 단계의 로직을 구현 및 변경하는 방법으로 성능 향상.

  

- 반정규화를 적용

	- 반정규화 대상으로는 테이블, 속성, 관계에 대해 적용, 중복을 통한 방법만이 반정규화가 아니며

	  테이블, 속성, 관계를 추가,분할,제거 할 수 있다.



#### 2. 반정규화 기법      

- 테이블 반정규화

	- 테이블 병합 

		- 1:1 관계 테이블 병합 : 1:1 테이블을 병합하여 성능 향상

		- 1:M 관계 테이블 병합 : 1:M 테이블을 통합하여 성능 향상

		- 슈퍼/서브 테이블 병합 : 슈퍼/서브 관계를 통합하여 성능향상.

	  

	- 테이블 분할

		- 수직분할 : 컬럼단위의 테이블을 1:1로 분리하여 성능향상

		- 수평분할 : Row 단위로 집중 발생되는 트랜잭션을 분석하여 테이블을 분할

   

	- 테이블 추가

		- 중복테이블 추가 : 다른 업무 또는 서버가 틀린경우 동일한 테이블 추가 (원격 조인 제거)

		- 통계테이블 추가 : 집계함수 등을 미리 수행하여 계산해둔 테이블을 추가 ( 쿼리 수행시에 계산하지 않는다.)

		- 이력테이블 추가 : 마스터 테이블에서 자주 조회되는 레코를 중복하여 테이블 추가 ( 범위처리 최소화)

		- 부분테이블 추가 : 자주 이용하는 집중화된 칼럼이 있는경우 디스크I/O를 줄이기위해 별도의 컬럼만 테이블 추가



- 컬럼 반정규화

	- 중복칼럼 추가 : 조인시 성능저하를 예방하기위해, 중복되는 컬럼을 위치시킴

	- 파생컬럼 추가 : 트랜잭션이 처리되는 시점에 계산에 의해 발생되는 성능저하를 예방하기 위해 미리 계산하여 컬럼에 보관

		( 미리 계산한 컬럼 sum , sumtoValue)

	- 이력테이블 컬럼 추가 : 대량의 이력데이터 처리시 기능성컬럼(최근값, 시작일자, 종료일자) 를 추가한다.



- 관계 반정규화

	- 중복관계 추가 : 여러경로를 거쳐 조인 할 수 있지만, 성능저하를 예방하기위해 추가적인 관계를 맺는 방법(중복 FK)

	***(테이블,컬럼 반정규화는 데이터 무결성에 영향을 끼치지만, 관계의 반정규화는 데이터의 무결성을 깨뜨리지 않는다 ) ***



