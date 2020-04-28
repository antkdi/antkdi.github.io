---
title: "SpringBoot로 완성하는 URL Shortener (1)"
comment: true
date: 2020-04-26
categories: [Programing, Java]
tags: [URL-Shortener, SpringBoot]
---

## SpringBoot로 완성하는 URL Shortener (1)  

#### 1. URL 주소 줄이기 , Url-Shortener란 ?

유투브나 또는 카페의 게시물을 친구에게 공유해 본 경험이 있다면 엄청난 URL 길이를 보고

놀랐던 경우가 있을 것이다. 이런 URL이 길 수 밖에 없는 원인은 URL은 페이지가 동작을 하게

만드는 많은 정보들이 같이 포함 되어 있기 때문이다.

이러한 URL을 축약 해주는 서비스들이 바로 Url Shortener라고 할 수 있다.



#### 2. URL을 축약 해주는 여러 서비스들

- ~~구글 - https://goo.gle/~~  (서비스 중단) 
- 비틀리 - http://bitly.kr/ 
- Short Url - https://www.shorturl.at/
- 등 너무 많음..



#### 3. 서비스를 하는 이유

그렇다면 이들은 어떤 이유로 이런 서비스를 제공 하는지 알아 볼 필요가 있다.

URL 축약 서비스는 무료료 서비스 되고 있다. URL 축약서비스의 기본적인 개념이

Url을 짧게 만들어서 서비스 주체가 되는도메인의 하위에 축약된 URL에 원래의 URL로 

리다이렉트하는 기능이 주가 되기 때문에 URL을 축약하고 축약된 URL을 공유하면 공유 받은 모든

사람들이 서비스 주체가 되는 도메인의 URL을 거쳐서 원래의 페이지로 이동한다. 

이러한 축약 사이트들이 위와 같은 공유  과정에서 엄청난 정보를 획득(접속/통계 등) 할수 있을 것이다.



#### 4. Url Shortener 개념 알기 

![Url-Shortener-Diagram](https://user-images.githubusercontent.com/5414251/80403196-3031e800-88fa-11ea-95b6-7a6e61ed07d6.png)



위 다이어 그램과 같은  방식의 처리를 진행

- 첫번째 URL 축소하는 방법
  - 실제 URL을 축소시키는 방법이 아닌 , URL을 저장하고 저장된 URL의 시퀀스를 인코딩
- URL 을 리다이렉트 하는 방법
  - 도메인 하위에 빈 껍데기 HTML을 만들고 헤더에 리다이렉트 명령을 사용





#### 5.마치며..

이번에는 UrlShortener의 개념을 알아보았습니다.

다음에는 실제 코드작성을 통한 포스팅을 하겠습니다.

`Spring-boot`, `H2`, `JPA/Hibernate` 를 이용해서 만들어 보겠습니다.

먼저 코드를 확인 하실분은 [Github](https://github.com/antkdi/url-shortener) 를 확인 해주세요.
