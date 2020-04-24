---
title: "Oracle EXISTS 와 IN 연산자 바로 알기"
comment: true
date: 2019-07-15
categories: [DBMS, Oracle]
tags: [EXISTS, NOT IN, SQL]

---

## Oracle EXISTS 와 IN 연산자 바로 알기


- `TEST1`

| NO   | NAME  |
|:----:|:-----:|
| 1    | KIM   |
| 2    | LEE   |
| 3    | PARK  |
|      | JANG  |

 - `TEST2`
 
| NO   | NAME  |
|:----:|:-----:|
| 1    | KIM   |
| 2    | LEE   |
| 3    | PARK  |
|      | JANG  |
| 4    | JUNG  |

위 테이블이 있다고 가정 했을때, 아래 쿼리의 결과는 없다.

``` sql
SELECT * FROM TEST1 A 
WHERE A.NO NOT IN ( SELECT NO FROM TEST2 );
```

NOT IN 의 경우 서브쿼리의 행이 NULL 이 있는경우 조건자체가 FALSE 처리 되기 때문.


``` sql
SELECT * FROM TEST1 A

WHERE A.NO NOT EXISTS ( SELECT 1 FROM TEST2 WHERE A.NO = B.NO );
```


이쿼리의 결과는 A.NO = B.NO 인 것들을 제외하고 노출

이유는 EXISTS 연산자의 경우 <U>값의 존재만 체크하고 실제 값까지 확인 하지 않기때문에</U> NULL 인지 알수 없어서..




추가로 발견한 것이 있다면..



EXISTS 연산자의 경우 조인이 되지 않는다면 TRUE FALSE 만을 반환해서 쿼리 수행 자체에 영향을 미친다는 것이다.


``` sql
SELECT * FROM TEST1 WHERE EXISTS ( SELECT * FROM TEST1 WHERE NAME ='SONG' );
```

이경우에 서브쿼리의 결과가 존재하지 않기 때문에 FALSE 를 반환하고 결과는 나오지 않음.


``` sql
SELECT * FROM TEST1 WHERE NOT EXISTS ( SELECT * FROM TEST1 WHERE NAME='SONG');
```
이경우는  서브쿼리의 결과가 FALSE 를 반환하지만 NOT EXISTS 연산이기 때문에 모든 리스트가 출력 ..

