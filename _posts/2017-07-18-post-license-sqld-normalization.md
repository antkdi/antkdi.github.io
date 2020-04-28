---
title: "SQLD 정규화 (Normalization) 요약"
comment: true
date: 2017-07-18
categories: [License, SQLD]
tags: [Normalization]

---



제 2장 2절 정규화와 성능


1. 정규화를 통한 성능 향상 전략

-데이터 중복제거와 분류

- 일반적으로 정규화를 하면 '입력/수정/삭제' 성능은 향상되고,
- '데이터조회' 성능은 향상 될 수도 저하될 수도 있다.

2. 반정규화된 테이블의 성능저하 사례1
<p style="text-align: left; clear: none; float: none;">
  <span class="imageblock" style="display:inline-block;width:00px;width: 600px; height: 465px;;height:auto;max-width:100%">
    <span data-url="https://t1.daumcdn.net/cfile/tistory/231CA34955A9ED5216?download" data-lightbox="lightbox">
      <img src="https://t1.daumcdn.net/cfile/tistory/231CA34955A9ED5216" style="cursor: pointer;max-width:100%;height:auto" width="600" height="365" filename="성능저하사례1.jpg" filemime="image/jpeg" style="width: 400px; height: 365px;" />
    </span>
  </span>
 </p>
 <p style="text-align: left; clear: none; float: none;"><br /></p><p style="text-align: left; clear: none; float: none;"><br /></p><p style="text-align: left; clear: none; float: none;">
  <span style="line-height: 16.3636360168457px;">&nbsp; &nbsp; 3</span>
  <span style="line-height: 16.3636360168457px;">. 반정규화된 테이블의 성능저하 사례</span>
  <span style="line-height: 16.3636360168457px;">2</span></p><p style="text-align: left; clear: none; float: none;">
  <span class="imageblock" style="display:inline-block;width:600px;width:600px; height: 285px;;height:auto;max-width:100%">
    <span data-url="https://t1.daumcdn.net/cfile/tistory/220C7A4955A9ED541E?download" data-lightbox="lightbox">
      <img src="https://t1.daumcdn.net/cfile/tistory/220C7A4955A9ED541E" style="cursor: pointer;max-width:100%;height:auto" width="600" height="485" filename="성능저하사례2.jpg" filemime="image/jpeg" style="width: 400px; height: 285px;" />
    </span>
  </span>
</p>
<p style="text-align: left; clear: none; float: none;">
	<br />
</p>
<p style="text-align: left; clear: none; float: none;">
	<br />
</p>
<p style="text-align: left; clear: none; float: none;">
	<span style="line-height: 16.3636360168457px;">&nbsp; &nbsp; 4</span><span style="line-height: 16.3636360168457px;">. 반정규화된 테이블의 성능저하 사례3</span>
</p>
<p style="text-align: left; clear: none; float: none;">
	<span class="imageblock" style="display:inline-block;width:600px;width: 600px; height: 423px;;height:auto;max-width:100%">
    	<span data-url="https://t1.daumcdn.net/cfile/tistory/222E0D4955A9ED560D?download" data-lightbox="lightbox">
        	<img src="https://t1.daumcdn.net/cfile/tistory/222E0D4955A9ED560D" style="cursor: pointer;max-width:100%;height:auto" width="600" height="423" filename="성능저하사례3-1.jpg" filemime="image/jpeg" style="width: 600px; height: 423px;" />
        </span>
    </span>
</p>
<p>
	<br />
</p>
<p style="text-align: left; clear: none; float: none;">
	<span class="imageblock" style="display:inline-block;width:600px;width: 600px; height: 231px;;height:auto;max-width:100%">
    	<span data-url="https://t1.daumcdn.net/cfile/tistory/2315294955A9ED5819?download" data-lightbox="lightbox">
        	<img src="https://t1.daumcdn.net/cfile/tistory/2315294955A9ED5819" style="cursor: pointer;max-width:100%;height:auto" width="600" height="231" filename="성능저하사례3-2.jpg" filemime="image/jpeg" style="width: 600px; height: 231px;" />
        </span>
    </span>
</p>
<p style="text-align: left; clear: none; float: none;">
	<br />
</p>
<p style="text-align: left; clear: none; float: none;">
	<br />
</p>
<p style="text-align: left; clear: none; float: none;">
	<span style="line-height: 16.3636360168457px;">&nbsp; &nbsp; 5</span>
    <span style="line-height: 16.3636360168457px;">. 반정규화된 테이블의 성능저하 사례4</span>
</p>
<p style="text-align: left; clear: none; float: none;">
	<span class="imageblock" style="display:inline-block;width:600px;width: 500px; height: 493px;;height:auto;max-width:100%">
    	<span data-url="https://t1.daumcdn.net/cfile/tistory/23249D4955A9ED5A13?download" data-lightbox="lightbox">
        	<img src="https://t1.daumcdn.net/cfile/tistory/23249D4955A9ED5A13" style="cursor: pointer;max-width:100%;height:auto" width="600" height="493" filename="성능저하사례4.jpg" filemime="image/jpeg" style="width: 600px; height: 493px;" />
        </span>
    </span>
</p>
<p>
</p>
<p>
	<span style="line-height: 16.3636360168457px;"><br /></span></p><p>&nbsp; &nbsp; 6. 함수적 종속성(Functional Dependency)에 근거한 정규화 수행 필요</p><p>&nbsp; &nbsp;&nbsp;</p><p>&nbsp; &nbsp; &nbsp; &nbsp; - 데이터들이 어떤 기준값(결정자: Deteminant)에 의해 종속(종속자:Dependent) 되는 현상</p><p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ex) 결정자 -&gt; 종속자/주민등록번호 -&gt;출생지,주소</p><p><br /></p><p><br /></p><p><br /></p><p style="line-height: 16.3636360168457px;"><span style="font-size: 12pt;"><b>정규화를 쉽게 이해하기 위해 펌글 (도움이 많이 되었던.. )</b></span></p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;">&nbsp;1. 제1 정규형 (1NF : First Normal Form)</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 모든 속성은 반드시 하나의 값을 가져야한다.</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - M : N 관계를 1 : M 관계로 변환</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp;&nbsp;</p><p style="line-height: 16.3636360168457px;">&nbsp;&nbsp;</p><p style="line-height: 16.3636360168457px;">&nbsp;2. 제2 정규형 (2NF : Second Normar Form)</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 모든 속성은 반드시 기본키 전부에 종속 되어야 한다.</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 함수의 부분 종속을 분리</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; (기본키에 의해 함수적 종속성을 가지고 있지 않는 것들을 분리 )</p><p style="line-height: 16.3636360168457px;">&nbsp;</p><p style="line-height: 16.3636360168457px;">&nbsp;3. 제3 정규형 (3NF : Third Normal Form)</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 기본키가 아닌 모든 속성 간에는 서로 종속될수 없다.</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 모든 속성들이 기본키에 이행적 함수 종속이 아니다.</p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;">&nbsp;3. 보이스.코드 정규형 (BCNF&nbsp;: Boyce/Codd Normal Form)</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 복잡한 식별자 관계에 의해 발생하는 문제를 해결하기 위해 제 3 정규형을 보완</p><p style="line-height: 16.3636360168457px;">&nbsp; &nbsp; - 모든 결정자가 후보키이다 ( 후보키가 아닌 것들을 제외 및 분리)</p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;">예시</p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;">비정규 관계의 형태&nbsp;</p><p style="line-height: 16.3636360168457px;"></p><p></p><p><table class="txc-table" width="74" cellspacing="0" cellpadding="0" border="0" style="border: none; border-collapse: collapse; font-family: 돋움; font-size: 12px; width: 74px;"><tbody><tr><td style="width: 73px; height: 24px; border: 1px solid rgb(204, 204, 204);"><p>&nbsp;주문번호</p></td>
</tr>
<tr><td style="width: 73px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-left-width: 1px; border-left-style: solid; border-left-color: rgb(204, 204, 204);"><p>&nbsp;주문일자</p><p>&nbsp;품목코드</p><p>&nbsp;품목단가</p><p>&nbsp;주문수량</p><p>&nbsp;고객번호</p><p>&nbsp;고객명</p><p>&nbsp;고객주소<span style="line-height: 16.3636360168457px; font-size: 9pt;">&nbsp;&nbsp;</span></p></td></tr></tbody></table><br />테이블로 나타내면</p><p><table class="txc-table" width="744" cellspacing="0" cellpadding="0" border="0" style="border: none; border-collapse: collapse; font-family: 돋움; font-size: 12px; width: 744px;"><tbody><tr><td style="width: 97px; height: 24px; border: 1px solid rgb(204, 204, 204);"><p style="text-align: center;">&nbsp;주문번호(PK)</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">주문일자&nbsp;</p></td>
<td style="width: 100px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">품목코드&nbsp;</p></td>
<td style="width: 103px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">품목단가&nbsp;</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">주문수량&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">고객번호&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">고객명&nbsp;</p></td>
<td style="width: 86px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-top-width: 1px; border-top-style: solid; border-top-color: rgb(204, 204, 204);"><p style="text-align: center;">고객주소&nbsp;</p></td>
</tr>
<tr><td style="width: 97px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-left-width: 1px; border-left-style: solid; border-left-color: rgb(204, 204, 204);"><p style="text-align: center;">&nbsp;100</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">20141018&nbsp;</p></td>
<td style="width: 100px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">AA11&nbsp;</p></td>
<td style="width: 103px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">50000&nbsp;</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">1&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">1&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">홍길동&nbsp;</p></td>
<td style="width: 86px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">서울&nbsp;</p></td>
</tr>
<tr><td style="width: 97px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-left-width: 1px; border-left-style: solid; border-left-color: rgb(204, 204, 204);"><p style="text-align: center;">&nbsp;100</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">20141018&nbsp;</p></td>
<td style="width: 100px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">BB11&nbsp;</p></td>
<td style="width: 103px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">35000&nbsp;</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">3&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">1&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">홍길동&nbsp;</p></td>
<td style="width: 86px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">서울&nbsp;</p></td>
</tr>
<tr><td style="width: 97px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-left-width: 1px; border-left-style: solid; border-left-color: rgb(204, 204, 204);"><p style="text-align: center;">&nbsp;200</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">20141019&nbsp;</p></td>
<td style="width: 100px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">BB22&nbsp;</p></td>
<td style="width: 103px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">20000&nbsp;</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">2&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">2&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">아무개&nbsp;</p></td>
<td style="width: 86px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">대전&nbsp;</p></td>
</tr>
<tr><td style="width: 97px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-left-width: 1px; border-left-style: solid; border-left-color: rgb(204, 204, 204);"><p style="text-align: center;">&nbsp;300</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">20141019&nbsp;</p></td>
<td style="width: 100px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">AA11&nbsp;</p></td>
<td style="width: 103px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">50000&nbsp;</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">5&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">3&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">김무식&nbsp;</p></td>
<td style="width: 86px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">대구&nbsp;</p></td>
</tr>
<tr><td style="width: 97px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204); border-left-width: 1px; border-left-style: solid; border-left-color: rgb(204, 204, 204);"><p style="text-align: center;">&nbsp;400</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">20141019&nbsp;</p></td>
<td style="width: 100px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">AA11&nbsp;</p></td>
<td style="width: 103px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">50000&nbsp;</p></td>
<td style="width: 88px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">2&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">4&nbsp;</p></td>
<td style="width: 90px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">둘리&nbsp;</p></td>
<td style="width: 86px; height: 24px; border-bottom-width: 1px; border-bottom-style: solid; border-bottom-color: rgb(204, 204, 204); border-right-width: 1px; border-right-style: solid; border-right-color: rgb(204, 204, 204);"><p style="text-align: center;">부산&nbsp;</p></td>
</tr>
</tbody></table><br /></p><p>위 테이블에서 <u><b>주문번호 100 </b></u>은 1차 정규형 정의 중&nbsp;</p><p>&nbsp;<span style="line-height: 16.3636360168457px; font-size: 9pt;">&nbsp;</span><span style="line-height: 16.3636360168457px; font-size: 9pt;"><b>- 모든 속성은 반드시 하나의 값을 가져야한다.</b></span></p><p>를 위배되는 형태인 주문번호 가 문제가 되므로 이 테이블은<b> 1차 정규화의 대상</b>이 된다.</p><p>이것을 1차 정규화 하게 되면</p><p style="text-align: left; clear: none; float: none;"><br /></p><p style="text-align: left; clear: none; float: none;"><span class="imageblock" style="display:inline-block;width:389px;;height:auto;max-width:100%"><img src="https://t1.daumcdn.net/cfile/tistory/2274C03755A9F70E21" style="max-width:100%;height:auto" width="389" height="131" filename="1차정규화.png" filemime="image/jpeg"/></span></p><p style="text-align: left; clear: none; float: none;"><br /></p><p style="text-align: left; clear: none; float: none;">이러한 형태가 된다. 이해가 가는가 ?</p><p style="text-align: left; clear: none; float: none;">두개의 속성이 있는 애는 이제 없어졌다.</p><p style="text-align: left; clear: none; float: none;"><br /></p><p style="text-align: left; clear: none; float: none;">하지만 이 상황에서 2차정규형의 정의에 위배되는 사항이 존재한다.</p><p style="text-align: left; clear: none; float: none;"><br /></p><p style="line-height: 16.3636360168457px;"><b><u>2. 제2 정규형 (2NF : Second Normar Form)</u></b></p><p></p><p style="line-height: 16.3636360168457px;"><b><u>&nbsp; &nbsp; - 모든 속성은 반드시 기본키 전부에 종속 되어야 한다.</u></b></p><p></p><p style="line-height: 16.3636360168457px;"><b><u>&nbsp; &nbsp; - 함수의 부분 종속을 분리</u></b></p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;">주문품목 테이블은 품목단가는 주문번호(FK)에 종속적이지 않다.</p><p style="line-height: 16.3636360168457px;">오히려 품목코드에 종속적인 것이 품목단가이다.</p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;">2차 정규화의 규칙에 의해 분리를 하고나면,</p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;"><span class="imageblock" style="display:inline-block;width:588px;;height:auto;max-width:100%"><img src="https://t1.daumcdn.net/cfile/tistory/2636813655A9F81533" style="max-width:100%;height:auto" width="588" height="134" filename="2차정규화.png" filemime="image/jpeg"/></span></p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;"></p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;">이 형태가 나온다</p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;"><br /></p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;">이것이 2차정규화를 마친 테이블이다.</p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;"><br /></p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;">3차정규형을 보자..</p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;"><br /></p><p style="line-height: 16.3636360168457px; text-align: left; clear: none; float: none;">위의 테이블 관계는 3 차정규형을 위배하는 것이 존재한다.</p><p style="line-height: 16.3636360168457px;"><br /></p><p style="line-height: 16.3636360168457px;"><b><u>3. 제3 정규형 (3NF : Third Normal Form)</u></b></p><p style="line-height: 16.3636360168457px;"><b><u>&nbsp; &nbsp; - 기본키가 아닌 모든 속성 간에는 서로 종속될수 없다.</u></b></p><p style="line-height: 16.3636360168457px;"><b><u>&nbsp; &nbsp; - 모든 속성들이 기본키에 이행적 함수 종속이 아니다.</u></b></p><p></p><p><br /></p><p>주문 테이블을 보면,</p><p>고객명과 고객주소는 고객번호에 종속된다는 것을 쉽게 알수 있다.</p><p>기본키가 아닌 속성간에는 서로 종속될수 없기 때문에 이것을 분리해 주어야함.</p><p>분리해주면,</p><p><br /></p><p style="text-align: left; clear: none; float: none;"><span class="imageblock" style="display:inline-block;width:609px;;height:auto;max-width:100%"><img src="https://t1.daumcdn.net/cfile/tistory/2761103B55A9F9811E" style="max-width:100%;height:auto" width="609" height="245" filename="3차정규화.png" filemime="image/jpeg"/></span></p><p><br /></p><p>비로소 3차 정규화 까지 마치게 되는 것이다.</p><p><br /></p><p>이해가 잘 안간다면&nbsp;<a href="http://blog.naver.com/mjsolar/130109454313" target="_blank" class="tx-link">http://blog.naver.com/mjsolar/130109454313</a></p><p>을 보고 이해하길 바란다..&nbsp;</p><p><br /></p><p>SQLD 를 공부하는 모든 사람이 정규화의 늪에 빠지지 않도록 열심히 공부하자.</p><p><br /></p><p><br /></p><p><br /></p><p><br /></p><p><br /></p><p></p><p style="line-height: 16.3636360168457px;"><br /></p><div class="container_postbtn #post_button_group"><div class="postbtn_like"><div class="wrap_btn"><button type="button" class="btn_post like_btn uoc-icon" data-uoc-svc="tistory" data-uoc-uid="1899281_11" data-uoc-sc="" data-uoc-pcUrl="https://antkdi.tistory.com/11" data-uoc-fetchurl="http://api.kakao.tistory.com/like/fetch?uid=1899281_11">






