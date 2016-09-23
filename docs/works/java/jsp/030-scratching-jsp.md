# JSP

2010.07.19

JSP EL, JSTL, JSF 모두 Brain Dead Design 의 전형인듯.
우째 일을 덜어주지 못하고 더 만들지.
게다가 활용구역은 한정적이고.

<% %> 밖에 답이 없네.
10 년간 뭐했담.

M$ 도 이제 버리려고 하는 ASP.NET 이벤트 모델 배껴서 JSF 만들었던데,
걍 빨리 Razor 배껴서 JHTML 이나.

2010.08.08

슬릭에 사용할 JSP 스타일
EL ,,, 즐~

*

	<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
	<%@ page import="com.drypot.sleek.controller.*" %>
	<% HomeController cnt = (HomeController)request.getAttribute("cnt"); %>
	<!DOCTYPE html>
	<html>
	<body>

	<%= cnt.controllerName %><br>
	<%= cnt.now %><br>
	<%= cnt.rendered %>

	</body>
	</html>

2010.08.09

클로저 없으니 진짜 깝깝하다. ㅋㅋ
한 줄로 될꺼 클래스 뽑으면서 열 줄 넘게 만들어야하네. =,=

Scala + Scalate

	= pager.html(p => url.postList(page = p))

JSP

	<%= HtmlUtil.renderPager(out, cnt.pager, new Func<Integer, String>() { public String apply(Integer page) { return cnt.urls.getPostListUrl(page); }} ) %>

2010.10.26

EL 에 적응하려고 며칠 노력했음.
진심임.

EL 로 부족하면 SpEL 로 때우려고 SpEL 도 봤음.
그러나 너무 후져서, 절망하고,

정신차리고 이클립스로 돌아와서,
멍하니 에디터 보다가,
EL 싹다 들어냈음.
쌩 Java 코드로 다 치환.

코드량은 두배 되지만,
먼가 명확해지고 속이다 후련하네.

어느 똑똑한 용자가 Razor Java 버전 만들 때까지 이리 버텨야지.

2010-11-29

JSP 에 코드가 있어야해.

프리젠테이션 관련 로직이 컨트롤러에 있으면 문제가 너무 복잡해짐.
프리젠테이션 관련 로직은 JSP 에 있어야해.

형식상으로 Java / JSP 나눠어서 코드를 모두 컨트롤러에 JSP 에서는 출력만,, 이렇게 나누는 것 보다
의미상으로 컨트롤 로직 / 뷰 로직으로 나누는 것이 명쾌함.

이클립스 처럼 JSP 편집기가 꼬지면 Java 코드는 Java 파일에 두고 싶을텐데,
IDEA 처럼 Java / JSP 편집 능력에 구분이 없어지면 파일 포맷이 아니라 의미상으로 코드를 나눌 수 있게 됨.

아주 그냥 편하고,
컨트롤러에서 뷰로 데이터 보내느라 뻘짓 안 해도 됨.
