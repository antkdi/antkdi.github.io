---
title: "SQLD 대량 데이터에 따른 성능 (BigData) 요약"
comment: true
date: 2017-07-18
categories: [License, SQLD]
tags: [BigData]

---

### 제4절 대량 데이터에 따른 성능

#### 1. 대량 데이터 발생에 따른 테이블 분할 개요

**문제점**

- 대량의 데이터가 하나의 테이블에 집약되어 있고 하나의 하드웨어 공간에 저장되어 있으면 성능저하를 피하기 힘듬.

- 인덱스 또한 트리가 커지고 깊이가 깊어져 조회 성능에 영향을 미침

- 입력, 수정, 삭제 트랜잭션의 경우도 특성상 작업량이 증가하며 , 더많은 성능저하를 유발.

- 칼럼이 많아지면 물리적인 디스크의 여러 블록에 걸쳐 저장되므로 Row 길이가 길어 로우체닝과 로우 마이그레이션이 많아 짐 -> 성능저하

***********



#### 2. 한 테이블에 많은 칼럼을 가지고 있는 경우

- 200개의 컬럼을 가진 도서정보 테이블일 경우 하나의 로우 길이가 10K이고 블록이 2K로 쪼개져 있으면, 한 로우는 5개의 블록에 걸쳐 저장된다.

- 이런 경우 칼럼 분할 반정규화를 수행.

  

#### 3. 대량 데이터 저장 및 처리로 인한 성능

- 대량 데이터가 예상될때, 파티셔닝 및 PK 에 의해 테이블 분할 하는 방법을 적용할 수 있다.

- 오라클의 경우 List Partition, Range Partition, Hash Partition, Composite Partition 방법 가능



- Range Partition ( 가장 많이 사용하는 파티셔닝 기법)

	- 요금정보 처럼 항상 월단위로 데이터를 처리하는 특성을 가졌다면, 월별로 12개의 파티션테이블로 분할하여 성능개선 유도

	- 대상 테이블이 날짜 또는 숫자값으로 분리가 가능하고 각 영역별로 트랜잭션이 분리된다면 적용.

  

- List Partition

	- 지점, 사업소, 사업장 등 핵심적인 코드 값으로 PK 가 구성되거나 분류 할수 있는 테이블이라면 PK에 의해 파티셔닝이 되는

	- List Partition 을 적용 할 수 있다. 하지만, Range Partition 처럼 보관주기에 따라 쉽게 삭제하는 기능은 제공 되지 않음.

  

- Hash Partition

	- Hash 조건에 따라 해시알고리즘이 적용되어 테이블이 분리되므로 설계자는 데이터가 어떤 테이블에 들어갔는지 알수 없음.

- Composite Partition

	- Main/ Sub 관계로 나누어 분할하는 파티셔닝 기법.

  

   

#### 4. 테이블에 대한 수평분할/ 수직분할 절차

- 데이터 모델링을 완성 -> 데이터베이스 용량산정 -> 대량 데이터가 처리되는 테이블에 대해서 트랜잭션 처리패턴 분석

  -> 집중화된 트랜잭션이 컬럼,로우 등으로 발생하는 것을 파악하고 테이블을 분리 검토 -> 컬럼수가 많다면 1:1 분리 고려, 데이터

  용량이 많아 성능이 저하된다면 파티셔닝기법 적용 검토



