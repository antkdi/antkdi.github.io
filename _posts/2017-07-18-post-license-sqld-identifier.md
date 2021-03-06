---
title: "SQLD 식별자 (Identifier) 요약"
comment: true
date: 2017-07-18
categories: [License, SQLD]
tags: [Identifier]

---

### 제1장 5절 식별자 (Identifier)



#### 1. 식별자의 개념

- 엔터티의 여러 속성 중 엔터티를 대표하는 속성

- 엔터티는 반드시 하나의 유일한 식별자를 가진다

 > cf ) 식별자 vs PK
 > - 논리데이터 모델링 (식별자)
 > - 물리데이터 모데링 (PK)

#### 2. 식별자의 특성

- 유일성

	- 엔터티 내에 모든 인스턴스들을 유일하게 구분

- 최소성

	- 주식별자를 구성하는 속성의 수는 유일성을 만족하는 최소의수

- 불변성

	- 한번 정해지면 변하지 않아야 한다

- 존재성

	- NULL X

	

#### 3. 식별자 분류 및 표기법

- 대표성 여부

	- 주식별자

	- 보조식별자

- 스스로 생성여부

	- 내부식별자

	- 외부식별자

- 속성의 수

	- 단일 식별자

	- 복합 식별자

- 대체여부

	- 본질 식별자

	- 인조 식별자

	

#### 4. 주식별자 도출기준

- 해당 업무에 자주 쓰이는 속성이 주식별자

- 이름으로 기술되는 것은 피한다

	- WHERE 절에 정확한 기술이 어렵기 때문

- 복합 식별자 구성시 최대한 속성의 수를 줄여 쿼리를 단순하게 구성한다



#### 5. 식별자와 비식별자관계에 따른 식별자

- 식별자관계와 비식별자관계의 결정

	- 부모 엔터티로부터 받은 외부식별자를 자신의 주식별자로 사용할 것인지,

	  연결만 되는 속성으로만 사용할 것인지 결정해야 한다.

- 식별자관계

	- 자식엔터티의 주식별자로 부터 부모엔터티의 주식별자가 상속되는경우

	- 자식엔터티가 부모엔터티로 부터 받은 식별자만이 자신의 주식별자라 하면 1:1 관계이고,

	  다른 부모엔터티로 부터 받은 식별자도 자신의 주식별자로 사용하거나 내부속성과 함께 사용하면 1:M 관계가된다.

- 비식별자 관계

	- 부모로 부터 받은 속성이 필수가 아닐때

	- 데이터의 생명주기에 의해 부모가 자식만 남겨두고 소멸해 버릴때

	- 부모로 부터 받은 속성을 주식별자로 사용해도 되지만 별도의 주식별자 생성이 더 유리할 때

- 식별자관계로만 할 경우 문제점

	- 주식별자의 속성 수가 증가하여 WHERE 절이 길어지고 쿼리가 복잡해진다.

- 비식별자 관계로만 할 경우 문제점

	- 하나의 데이터 검색을 위해 쓸데없이 부모까지 찾아야한다

	- 많은 JOIN문으로 인해 복잡성이 증가하여 성능이 저하

- 비식별자와 비식별자관계 모델링

	관계분석 --> 관계의 강/약분석 --> 자식 테이블 독립PK 필요 --> 쿼리복잡도 증가, 생산성하향

	- 관계의 강/약 분석이 약인경우 비식별자관계고려

	- 자식테이블이 독립PK 가 필요한 경우 비식별자 관계 고려

	- 쿼리가 복잡하고 생상성이 낮으면 비식별자 관계 고려







