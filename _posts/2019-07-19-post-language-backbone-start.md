---
title: "Backbone.js 시작하기"
comment: true
date: 2019-07-19
categories: [Programing, Backbone.js]
tags: [Getting Started]

---

### Backbone.js 시작하기

안녕하세요.
오늘은 Backbone.js 에 대한 포스팅을 하겠습니다.

SPA(Single Page Application)가 대세가 된 오늘날의 프론트엔드는
폼체크를 하고, 인풋박스를 만들고 서브밋을 시키는 것 등등..
예전과는 판이하게 다른 난이도를 가지고 있고,
그것을 jQuery나 javascript 로 선택자나 콜백 더미로 만들었다가는 유지보수도 힘들고,
정작 개발한 본인도 고생하는 일이 대 다수 입니다.

Backbone.js 가 제시하는 모델은 아래와 같습니다
![Backbone.js가 제시하는 모델](/images/post/backbone_start/model.png)

|Model |View|
| :--- | :--- |
|데이터 및 비즈니스 논리를 조율	| 변경시 UI 렌더링|
|서버에서 로드 및 저장	|사용자 입력 및 상호 작용|
|데이터가 변경되면 이벤트 발생	|캡처 된 입력을 모델로 전송|

#### 모델
>모델은 데이터 속성의 내부 테이블을 관리하고 데이터가 수정 될 때 "변경"이벤트를 트리거합니다. 모델은 지속성 계층 (일반적으로 백업 데이터베이스가있는 REST API)과 데이터 동기화를 처리합니다. 특정 데이터 비트를 조작하는 데 유용한 모든 기능을 포함하는 재사용 가능한 원자 적 개체로 모델을 디자인하십시오. 모델은 앱 전체에서 전달 될 수 있어야하며 데이터가 필요한 곳이면 어디에서나 사용할 수 있어야합니다.

#### 뷰
>뷰(View)는 사용자 인터페이스의 원자 덩어리입니다. 특정 모델 또는 여러 모델의 데이터를 렌더링하는 경우가 많지만 뷰는 데이터가없는 UI 혼자있을 수 있습니다. 모델은 일반적으로 뷰를 인식하지 않아야합니다. 대신 뷰는 모델 "변경"이벤트를 수신하고 적절하게 대응하거나 다시 렌더링합니다.

다음은 Backbone.js의 컬렉션 모델입니다.
![컬렉션 모델](/images/post/backbone_start/collection.png)

>컬렉션을 사용하면 관련 모델 그룹을 처리하고 서버에 새 모델로드 및 저장을 처리하고 모델 목록에 대해 집계 또는 계산을 수행하는 도우미 함수를 제공 할 수 있습니다. 자신의 이벤트 외에도 컬렉션은 발생하는 모든 이벤트를 모델 내부로 모델링하므로 컬렉션의 모든 모델에서 발생할 수있는 변경 사항을 한 곳에서들을 수 있습니다.

>백본은 RESTful API와 동기화되도록 미리 구성되어 있습니다. 리소스 엔드 포인트의 URL로 새 콜렉션을 작성하기 만하면됩니다.

백본은 RESTful API와 동기화되도록 미리 구성되어 있습니다. 리소스 엔드 포인트의 URL로 새 콜렉션을 작성하기 만하면됩니다.

``` javascript
var Books = Backbone.Collection.extend ({
  url : '/ books'
});
```

콜렉션 및 모델 구성 요소는 함께 다음 메소드를 사용하여 REST 자원의 직접 맵핑을 형성합니다.
``` javascript
GET / books / .... collection.fetch ();
POST / books / .... collection.create ();
GET / books / 1 ... model.fetch ();
PUT / books / 1 ... model.save ();
DEL / books / 1 ... model.destroy ();
```

위와 같은 개념을 알고 다음시간에는 의존성 파일을
직접 임포트 해서 최소한의 예제를 만들어 보겠습니다.

감사합니다.
