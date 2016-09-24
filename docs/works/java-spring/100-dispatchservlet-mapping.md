---
title: DispatcherServlet Mapping
---

2010-10-25

### DispatcherServlet 매핑 문제

Spring MVC 가 해석하는 이쁜 URL 과 Servlet Spec 의 URL Pattern 매칭과 쫑나기 때문에 골치가 아파집니다.

`DispatcherServlet` 을 `/spring/*` 나 `*.do` 같은 식의 안 이쁜 Servlet URL Pattern 에 매칭하면
말 그대로 이쁘지 않습니다.

`DispatcherServlet` 을 `/spring/*` 에 매칭시키고 URL Redirect Filter 를 써서
겉으로 이쁜 URL 을 안 이쁜 URL 로 매핑하면 URL Redirect Filter 를 써야합니다.
번거롭습니다.

원래는 톰켓의 `DefaultServlet` 이 `/` 에 매핑 되어 스터틱 파일을 서비스하게 되어있는데
`DispatcherServlet` 을 `/` 디폴트 서블릿으로 등록하고 스프링 파이프라인을 먼저 통과하게 할 수도 있습니다.
설정이 비교적 간단하고 꽤 문제 없어 보입니다.
하지만 일반 파일도 스프링 파이프라인을 통과하기 때문에 먼가 오버헤드 스럽고 기분이 나빠집니다.

### 꽁수

그래서 꽁수를 냈습니다.

`DispatcherServlet` 을 `/` 디폴트 패턴에 등록하고
서비스 예상가능한 모든 스터틱 파일들을 `*.xxx` 식 확장자 URL Pattern 으로 등록해주는 겁니다.
스프링은 스프링대로, 스터틱 파일들은 스터틱 파일대로 톰켓 수준에서 깔끔하게 분리되서 서비스 되면서
별도 필터도 필요 없어집니다.
단점은, 모든 확장자를 (최소한 많이 사용되는 확장자는) 모두 확장자 패턴으로 등록해줘야합니다.
그런데 생각해보면 몇 개 없습니다. 제 경우는 `css`, `jpeg`, `js` 정도.

    <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>*.jpg</url-pattern>
      <url-pattern>*.css</url-pattern>
      <url-pattern>*.js</url-pattern>
      <url-pattern>*.ico</url-pattern>
    </servlet-mapping>

### Servlet 3.0 URL Path Mapping

servlet 3.0 스펙의 url path mapping 룰을 참고삼아 정리해 놓습니다.

아래 순서대로 검사되며 성공적인 첫 매칭에 따라 적용할 servlet 이 결정됩니다.  
일단 매칭되면 다음 검사 단계로 넘어가지 않습니다.

request path 와 servlet url-pattern 이 완전히 동일한 경우 최우선으로 선택합니다.

매칭되는 가장 긴 path-prefix 를 가진 servlet 검색.  
path-mapping 스타일의 servlet 은 url-pattern 이 '/' 로 시작해서 '/*' 으로 끝납니다.

확장자 패턴으로 등록한 servlet 검색.  
extension-mapping 스타일의 servlet 은 url-pattern 이 '*.' 으로 시작합니다.

위 방식 모두 실패하면 default servlet 사용.  
default servlet 을 표시하는 url-pattern 은 '/'.


{:class="go-to-index"}
[Java Spring](index)
