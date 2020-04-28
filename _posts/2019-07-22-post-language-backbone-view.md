---
title: "Backbone.js View 생성해보기"
comment: true
date: 2019-07-22
categories: [Programing, Backbone.js]
tags: [View]

---

### Backbone.js View 생성해보기

안녕하세요.
오늘은 `Backbone.js` View 에 대한 포스팅을 하겠습니다.

이전 포스트 에서 View에 대해서 인용하였던 것을 다시 보겠습니다.

#### 공식 홈페이지의 View 설명

>`Backbone.js` View는  규칙적입니다. 
HTML 또는 CSS에 대한 정보는 사용자가 결정하지 않으며 모든 JavaScript 템플릿 라이브러리에서 사용할 수 있습니다. 
일반적인 생각은 모델을 기반으로 논리적 뷰로 인터페이스를 구성하는 것입니다.
각 모델은 모델을 변경할 때 독립적으로 업데이트 할 수 있으며 페이지를 다시 그리지 않아도됩니다. 
JSON 객체를 파헤 치고 DOM에서 요소를 찾고 손으로 HTML을 업데이트하는 대신 뷰의 렌더링 함수를 모델의 "change"이벤트에 바인딩 할 수 있습니다. 이제 모델 데이터가 UI에 표시되는 모든 곳에서, 그것은 항상 최신의 데이터를 유지 합니다.

#### 지난 포스트의 View 개념
>뷰(View)는 사용자 인터페이스의 원자 덩어리입니다. 특정 모델 또는 여러 모델의 데이터를 렌더링하는 경우가 많지만 뷰는 데이터가없는 UI 혼자있을 수 있습니다. 모델은 일반적으로 뷰를 인식하지 않아야합니다. 대신 뷰는 모델 "변경"이벤트를 수신하고 적절하게 대응하거나 다시 렌더링합니다.

![Backbone.js가 제시하는 Model And View ](/images/post/backbone_start/model.png)

그러면 한번 코드를 작성해 볼까요.

Header와 Content 영역의 버튼을 만들고 각 버튼의 클릭이벤트를 작성 해보겠습니다.


``` javascript
<html>
	<head>
      <!-- 필요한 js Import -->
      <script src="js/jquery.min.js"></script>
      <script src="js/underscore-min.js"></script>
      <script src="js/Backbone.js"></script>
	</head>
 	<body>
      <div id="header">

      </div>
      <div id="content">

      </div>
    </body>
</html>
```


두개의 빈 Div가 있습니다.

Backbone.js의 View를 이용해서 버튼을 작성해 보겠습니다.

- 각 버튼의 View Template 작성


``` javascript

<html>
	<head>
      <!-- 필요한 js Import -->
      <script src="js/jquery.min.js"></script>
      <script src="js/underscore-min.js"></script>
      <script src="js/Backbone.js"></script>
	</head>
 	<body>
      <div id="header">

      </div>
      <div id="content">

      </div>
    </body>
</html>

<!-- 헤더 버튼 템플릿 -->
<script type="text/template" id="header_template">
    <button>Header Button</button>
</script>
<!-- 컨텐츠 버튼 템플릿 -->
<script type="text/template" id="content_template">
    <button>Content Button</button>
</script>
```


- 뷰 작성 하기


``` javascript
<script>

    var HeaderButtonView = Backbone.View.extend({
        el:$("#header"),
        template: _.template($("#header_template").html()),
        initialize: function (){
            this.render();
        },
        render: function(model) {
            $(this.el).html(this.template(this.model));
            return this;
        }
    });

    var ContentButtonView = Backbone.View.extend({
        el:$("#content"),
        template: _.template($("#content_template").html()),
        initialize: function (){
            this.render();
        },
        render: function(model) {
            $(this.el).html(this.template(this.model));
            return this;
        }
    });
    
</script>
```

위처럼 View는 작성하되 extends 받은 속성들을 Overriding 할 수 있는데요.

간략하게 설명 드리면,
- el : 뷰의 대상이 될 DOM
- template : 뷰의 템플릿 선언 
- initialize : 생성시 호출되는 콜백
- render: 렌더링 함수

위 내용으로 보아, 헤더 버튼과 컨텐츠 버튼은 각각 #header, #content 영역을 대상으로 렌더링되고 템플릿은 #header_template, #content_template 을 상용하며, 오브젝트 생성시 렌더링 되는 View 라는 것을 짐작 할 수 있습니다. 

여기까지는 아직 화면에 영역만 있을 뿐, 버튼이 나오지는 않습니다. 오브젝트 생성을 하지 않았기 때문이죠.

오브젝트를 생성해 봅시다.

``` javascript
<script>
  $(function () {
        var headerView = new HeaderButtonView();
        var contentView = new ContentButtonView();
    });
</script>
```

위처럼 스크립트의 내용중에 즉시실행함수로 오브젝트를 생성해 보았습니다.

이제 버튼 두개가 영역에 나타나게 됩니다.

따로 스타일은 작성하지 않았기 때문에 그냥 버튼 두개가 나타나는 것처럼 보입니다.

이제 생성된 오브젝트에 이벤트를 걸어 봅시다.


``` javascript
<script>

    var HeaderButtonView = Backbone.View.extend({
        el:$("#header"),
        template: _.template($("#header_template").html()),
        initialize: function (){
            this.render();
        },
        render: function(model) {
            $(this.el).html(this.template(this.model));
            return this;
        },
        events: {
            "click": function(){
                this.$el.find("button").text("Header!")
            }
        }
    });

    var ContentButtonView = Backbone.View.extend({
        el:$("#content"),
        template: _.template($("#content_template").html()),
        initialize: function (){
            this.render();
        },
        render: function(model) {
            $(this.el).html(this.template(this.model));
            return this;
        },
        events: {
            "click": function(){
               this.$el.find("button").text("Content!")
            }
        }
    });
    
</script>
```


추가된 것은 events 프로퍼티죠. 각 버튼의 Text를 변경하게 해보았습니다.

페이지를 새로고침하고 버튼을 클릭하면 버튼 TEXT가 변경되는 것을 볼 수 있습니다.

이렇듯 `Backbone.js` `View` 는 렌더링 영역 자체를 오브젝트로 생성하고 이벤트를 지원하며,

데이터가 변경 되었을때 즉시 데이터에 맞는 렌더링을 자유롭게 할 수 있는 장점이 있습니다.

이상으로 View를 알아 보았습니다.