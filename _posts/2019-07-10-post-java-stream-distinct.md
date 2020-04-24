---
title: "Stream API Distinct '중복제거'"
comment: true
date: 2019-07-10
categories: [Programing, Java]
tags: [Stream API, Distinct]
last_modified_at: 2019-07-10T18:45:25-05:00
---

### Predicate를 이용한 다중 속성 중복 제거.


```java

private static <T> Predicate<T> distinctByKeys(Function<? super T, ?>... keyExtractors) {
        final Map<List<?>, Boolean> seen = new ConcurrentHashMap<>();
        return t -> {
            final List<?> keys = Arrays.stream(keyExtractors)
                    .map(ke -> ke.apply(t))
                    .collect(Collectors.toList());
            return seen.putIfAbsent(keys, Boolean.TRUE) == null;
        };
    }
	
List<CareerCompanyVO> distinctCareer = Dictionary.careers.stream()
                .filter(x -> !filteringKeywords.contains(x.getCompany_nm()))
                .filter(distinctByKeys(CareerCompanyVO::getCompany_nm, CareerCompanyVO::getDept_nm))
                .collect(Collectors.toList());

```