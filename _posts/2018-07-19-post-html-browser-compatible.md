---
title: "브라우저 호환성 보기"
comment: true
date: 2018-07-19
categories: [Programing, Web]
tags: [HTML, Meta Tag]

---

## 브라우저 호환성 보기

사이트를 window7   IE10 환경에서  jsp를 이용해서 페이지를 구현했는데
 다른 컴퓨터에서는  이페이지의 스크립트가 안먹는다거나  테이블의 위치나 tr,td 등의 것들이 안먹는 일이 일어났다

 내컴퓨터같은 경우에도 보이는 페이지의 상태는     도구  -->  호환성보기 설정 --> 호환성보기에서 모든 웹이트표시 를 체크한다던가

 아니면 인터넷 주소창에 호환성그림을 클릭해서 활성화를 시켜야지 깨지지 않는 상황이 연출되었다

 

 즉  다른 사용자가 접속할때 IE 호환성보기설정을 설정해놓지 않으면은 깨지는게 다반사란 얘기였다.

 이부분을 모든 사용자가 접속해서 일괄적으로 같은 페이지를 보도록 설정했다

 
``` html
 <head>

 <meta http-equiv="X-UA-Compatible" content="IE=7">      --->  추가 코드

 </head>
```
 

여기서 유의할점은 꼭 head 바로 아래에 위문장을 집어넣으라는거다

 
``` html
 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">   ---->  원래 코드
```
 

원래 meta태그에는 이문장을 넣었었는데 지우고  위의 코드를 추가해주니...

웹 페이지가 뜰때부터 호환성보기가 IE7에 맞춰져서 include해서 사용하는 스크립트 코드가 원하는대로 적용되어 보여졌다

 

또한 페이지 주소표시줄 오른쪽에 호혼성 그림도 사라졌다

 

IE=7  익스플로러 7 환경으로 맞춰서 보여줘야 보이는듯하다.. IE=8로  설정해도 깨지는게 아닌가 ;;

사용하는 스크립트가 IE=7까지 적용이 되나보다

 

해당 IE버전으로 보여주는 듯하다

 ``` html

<meta http-equiv="X-UA-Compatible" content="IE=5">

<meta http-equiv="X-UA-Compatible" content="IE=6">

<meta http-equiv="X-UA-Compatible" content="IE=7">

<meta http-equiv="X-UA-Compatible" content="IE=8">

<meta http-equiv="X-UA-Compatible" content="IE=9">

 

가장 최신의 버전 모드는

<meta http-equiv="X-UA-Compatible" content="IE=Edge">

```
