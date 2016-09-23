# Wicket

2010.02.12

Wicket in Action 보고 있는 중

*

p12

For instance, with Wicket you’ll never code anything like the following (JSP) fragment:

	<table>
		<tr>
			<c:forEach var="item" items="${sessionScope.list}">
				<td>
					<c:out value="item.name" />
				</td>
			</c:forEach>
		</tr>
	</table>

Nor will you see code like the following Apache Velocity fragment:

	<table>
		<tr>
			#foreach ($item in $sessionScope.list)
				<td>
					${item.name}
				</td>
			#end
		</tr>
	</table>

Nor will it look like the following JSF fragment:

	<h:dataTable value="#{list}" var="item">
		<h:column>
			<h:outputText value="#{item.name}"/>
		</h:column>
	</h:dataTable>

With Wicket, the code looks like this:

	<table>
		<tr>
			<td wicket:id="list">
				<span wicket:id="name" />
			</td>
		</tr>
	</table>

만든 사람 철학이 좋은 프레임웍인 것 같음.
문제의 본질이 먼지 잘 알고 있는 듯.

2010.02.13

참 신기하게 동작함.
페이지 마다 오브젝트 트리를 만들어서 그걸 서버에서 계속 트래킹함.
페이지 별로 모든 버전을 다 만들어서 세션이 관리함. =,=

그래서 자바로 거의 Swing 급 프로그래밍을 해서 웹 사이트를 만들 수 있는데,
사용자가 다닌 모든 페이지 Object 를 서버가 관리함. =,=

예전 버전은 그걸 다 메모리에 가지고 있었나 본데,
최근 버전은 하드 디스크에 시리얼라이제이션 하나봄.
이게 먼 난리람... =,=

다 좋다가 이게 먼가... =,=

http://cwiki.apache.org/WICKET/page-maps.html
