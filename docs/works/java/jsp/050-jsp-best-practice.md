# JSP Best Practice

2010-11-29

JSP + Java 스타일 스크립팅을 하는데 베스트프랙티스는 다음과 같습니다.

제가 스프링란에 적은 대로 프로젝트에서 자주 사용하시는 Service 들을 기본으로 등록해주는
JSP Parent Class 를 만들어 페이지별 노가다를 최소로 줄입니다.

	public App app;
	public AuthSession authSession;
	public PostSession postSession;
	public RequestContext requestContext;

	public PostCategoryService postCategoryService;
	public PostService postService;
	public FileService fileService;

	public void jspInit() {
		super.jspInit();
		app = (App)springContext.getBean("app");
		authSession = (AuthSession)springContext.getBean("authSession");
		postSession = (PostSession)springContext.getBean("postSession");
		requestContext = (RequestContext)springContext.getBean("requestContext");
		postCategoryService = (PostCategoryService)springContext.getBean("postCategoryService");
		postService = (PostService)springContext.getBean("postService");
		fileService = (FileService)springContext.getBean("fileService");
	}

위와 같은 빈들 쫙 등록해 두면 편하겠지요.
기타 encode, format 등 자주 쓰는 메서드도 다 등록해버립니다.
이러면 JSTL 의 out, fmt:* 류 쓸 필요가 없어집니다.

2010-11-29

	<%@ page import="com.drypot.sleek.url.PostUrls" %>
	<%@ page import="com.drypot.sleek.domain.PostThread" %>
	<%@ page import="com.drypot.sleek.param.PostParams" %>
	<%@ page import="com.drypot.sleek.domain.PostCategory" %>
	<%@ page language="java" extends="com.drypot.sleek.AppJspBase" contentType="text/html; charset=UTF-8" %>
	<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>

	<%
		PostParams postParams = (PostParams)request.getAttribute("postParams");
		PostCategory postCategory = postSession.getPostCategory(postParams.getPostCategoryId());

		String title = postCategory.getName() + " - 새글";
		requestContext.setTitle(title);
	%>

	<jsp:include page="../page-begin.jsp"/>

	<div id="main">
		<div id="page-title"><%= encode(title) %></div>
		<form:form commandName="postForm" enctype="multipart/form-data">
			<div class="input"><form:errors path="*" cssClass="error-summary"/></div>

			<div class="label"><form:label path="post.userName" cssErrorClass="error">필명</form:label></div>
			<div class="input"><form:input path="post.userName" cssClass="tb" maxlength="32"/></div>

			<div class="label"><form:label path="postThread.title" cssErrorClass="error">제목</form:label></div>
			<div class="input"><form:input path="postThread.title" cssClass="tb" maxlength="128"/></div>

			<div class="label"><form:label path="post.text" cssErrorClass="error">본문</form:label></div>
			<div class="input"><form:textarea path="post.text" cols="48" rows="16"/></div>

			<div class="input"><div id="file"></div>
			<a id="addFile" href=".">파일 첨부란 추가</a>ShowShow</div>

			<div class="submit"><input class="submit" type="submit" value="완료"/></div>
		</form:form>
		<script>$(function(){initPostForm();});</script>
	</div>

	<jsp:include page="../page-end.jsp"/>

Spring + Hibernate 로 다시 만들고 있는 Sleek 새글쓰기 페이지 코드입니다.

page 디렉티브에서 extends="com.drypot.sleek.AppJspBase" 로 슈퍼클래스 지정하는 기능이 보입니다.
이러면 슈퍼클래스에서 등록한 스프링 빈과 유틸리티 메서드를 다 쓸 수 있게 됩니다.

컨트롤어에서 뷰로 보내는 인자들은 필드를 일일이 보내지 마시고, 코멘드 패턴으로 덩어리로 묶어서 보냅니다.
아래 PostParams  오브젝트 처럼 한방에 다 보내면 필드 5 가지를 나누어 보낼 필요가 없습니다.

	PostParams postParams = (PostParams)request.getAttribute("postParams");

기타 뷰에서만 필요한 뷰 오브젝트들은 컨트롤러에서 생성해서 Model 에 담아 넘기지 마시고 걍 뷰에서 만듭니다.
JSP 에 코드 나오는 것은 나쁜 일이 아닙니다.
Java 와 JSP 를 구분할 때 문법적으로 구분하지 말고, 코드의 의미상으로 구분해야 합니다.
뷰 로직은 뷰에 있는 것이 맞습니다.

	PostCategory postCategory = postSession.getPostCategory(postParams.getPostCategoryId());

유틸리티 슈퍼클래스에 encode 같은 메서드 만들어주면 아래 처럼 인코딩 코드가 간단해집니다.

	<%= encode(title) %>

JSTL 코드보다 더 간단하죠?
JSTL 에서는 <c:out value="${title}"/> 해야하는데 더 외계어 같습니다. =,=

스프링 커스텀 테그는 다 활용합니다.
이건 수작업 코드를 쓰면 더 복잡해집니다.
핵심은 JSTL 이 나쁜 부분은 억지로 쓰지 말고, 스프링 경우와 같이 더 유용하면 쓰시면 됩니다.

	<form:input path="post.userName" cssClass="tb" maxlength="32"/>

예를 더 보면, IF 같은 경우는 걍 이렇게,

	<% if (post.hasFiles()) { %>
		<div class="label">삭제할 파일</div>
		<div class="input">
			<form:checkboxes path="deleteFileNames" items="${postForm.post.fileNames}" delimiter="<br>"/>
		</div>
	<% } %>

For 루프 같이 코드와 HTML 이 섞여야 하는 부분은 섞되, 내부에서 최대한 분리 되게,

	<% for (PostThread t : postThreads) { %>
		<%
			CharSequence titleCss = "t" +
				(t.getUpdateDate().getMillis() > authSession.getLastVisit().getMillis() ? " tn" : "") +
				(t.getId() == postParams.getPostThreadId() ? " tc" : "");
			CharSequence titleUrl = postUrls.getPostViewUrl(-1, -1, t.getId());
			CharSequence lengthHtml = t.getLength() > 1 ? "<span class=\"l\">[" + (t.getLength() - 1) + "]</span>" : "";
		%>
		<tr>
		<td class="<%= titleCss %>">
			<a class="a2" href="<%= titleUrl %>"><span><%= encode(t.getTitle()) %></span><%= lengthHtml %></a>ShowShow
		</td>
		<td class="u"><%= encode(t.getUserName()) %></td>
		<td class="d"><%= formatTillMinute(t.getUpdateDate()) %></td>
		<td class="h"><%= t.getHit() %></td>
		</tr>
	<% } %>

스트링 더하는 연산한 결과를 String 으로 챙기지 마시고 CharSequence 인터페이스로 받으시면
String 으로 한번 더 복제할 필요가 없어지니 부하가 내려갑니다.

JSTL 대신 Java 코드를 그대로 쓰면 통합환경의 코드 어시스트가 동작하는 장점도 있습니다.
