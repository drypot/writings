---
title: Beans in JSP without EL
---

2010-10-28

### EL 보다 Native Java

전 부터 몇 회 JSP + JSTL + EL 에 적응하려고 노력했었는데
EL 이 간단할 때는 간단하지만, 자바 기능을 다 못쓰다 보니 깝깝하기도 하고
Java 문법으로 다 되는데 (제 눈에는)이상해 보이는 JSTL 구문을 또 외우는 것도 낭비 같아서
EL 없이 최대한 간단히 JSP 에서 Spring 을 쓰는 방법을 궁리해 봤습니다.

줄거리는 JSP 페이지를 생성하는데 사용하는 커스텀 베이스 클래스를 만들고
커스텀 베이스 클래스에서 자주 사용되는 스프링 빈 와이어링을 해주는 것입니다.
JSP 페이지에서는 그냥 쓰기만 하면 되도록.


### Usage First

    <%@ page language="java" extends="com.drypot.sleek.JspBase" contentType="text/html; charset=UTF-8" %>
    
    <!DOCTYPE html>
    <html>
    <body>
    <%= requestRing.getNow() %>
    </body>
    </html>

결과부터 보자면 위와 같이 사용할 수 있는 환경을 꾸미는 것입니다.
`requestRing` 은 자주 사용되는 오브젝트들을 모아 놓은 request scope spring bean 입니다.
JSP 페이지 내에서는 `requestRing` 을 정의하는 부분이 보이지 않습니다.
이 오브젝트의 와이어링은 `com.drypot.sleek.JspBase` 클래스에서 하게 되는데
그래서 `page` 디렉티브에 `extends` 어트리뷰트를 추가해줘야 합니다.

### JspBase

    package com.drypot.sleek;
    
    import java.io.IOException;
    
    import javax.servlet.ServletConfig;
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import javax.servlet.jsp.HttpJspPage;
    
    import org.springframework.context.ApplicationContext;
    import org.springframework.web.context.support.WebApplicationContextUtils;
    
    @SuppressWarnings("serial")
    public abstract class JspBase extends HttpServlet implements HttpJspPage {
    
      public ApplicationContext springContext;
      public SessionRing sessionRing;
      public RequestRing requestRing;
    
      @Override
      public String getServletInfo() {
        return "Jsp";
      }
    
      @Override
      public final void init(ServletConfig config) throws ServletException {
        super.init(config);
        jspInit();
      }
    
      public void jspInit() {
        springContext = WebApplicationContextUtils.getRequiredWebApplicationContext(getServletContext());
        sessionRing = (SessionRing)springContext.getBean("sessionRing");
        requestRing = (RequestRing)springContext.getBean("requestRing");
      }
    
      @Override
      public final void destroy() {
        jspDestroy();
      }
    
      public void jspDestroy() {
      }
    
      @Override
      public final void service(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
        _jspService(request, response);
      }
    
      public abstract void _jspService(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException;
    }

위 Jsp 페이지에서 사용한 `JspBase` 클래스입니다.
살짝 긴데, Jsp 스펙에 따른 뼈대 메서드들이 주이고 추가된 스프링 코드는 `jspInit` 의 3 줄이 전부입니다.

Jsp 는 스프링이 초기화된 후에 만들어지기 때문에 `@Autowired` 로 스프링 빈을 연결할 수 없습니다.
대신 위에처럼 `WebApplicationContextUtils` 으로 수작업 연결합니다.
저는 Session 관련한 `SessionRing` 과 Request Scope 관련한 `RequestRing` 두개 오브젝트를 사용했는데,
더 연결할 스프링 빈이 있다면 추가하면 될겁니다.

Jsp 에서 생성된 Servlet 오브젝트도 싱글턴이기 때문에 스프링 빈을 연결해 쓰는데는 문제가 없습니다.
하지만 Jsp 페이지 별로 오브젝트가 하나만 만들어지기 때문에 스프링 빈 외에 공유 불가능한 일반 필드를 사용하면 안 됩니다.

### ContextLoaderListener

Jsp 에서 생성된 Servlet 은 개념상 스프링의 DispatcherServlet 과 동급이기 때문에
Jsp 페이지에서 사용할 스프링 빈 오브젝트를 DispatcherServlet 의 Spring Context 에서 가져오기가 난감합니다.
일반 Jsp 페이지에서는 request 오브젝트를 DispatcherServlet 으로 부터 받기 때문에 스프링 컨텍스트를 받을 수 있지만,
위 JspBase 는 request 오브젝트가 없는 상태에서 자신을 초기화해야 합니다.

그래서, 예전 예에서 사용했던 DispatcherServlet 의 Context 대신에
웹어플리케이션에 글로벌한 Spring Context 를 만들어줘야 합니다.
먼가 일이 복잡해지는 듯한 느낌이 들긴합니다 =,=

    <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>
        /WEB-INF/spring-config.xml
      </param-value>
    </context-param>

위에 처럼 `ContextLoaderListener` 이벤트 헨들러를 `web.xml` 에 등록하면
지정한 spring config xml 파일로부터 메타 데이터를 읽어서 bean 을 생성합니다.
글로벌 스프링 컨텍스트를 사용하므로 서블릿 로컬한 컨텍스트 사용은 막아줍니다. 아래처럼.
(안 막으면 파일 못찾는다고 에러 냅니다)

    <servlet>
      <servlet-name>spring</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value></param-value>
      </init-param>
      <load-on-startup>3</load-on-startup>
    </servlet>

### Dependencies

HttpJspBase 클래스 관련 디펜턴시를 위해 tomcat-jsp-api 연결해주어야 합니다.

    <properties>
      <tomcat.version>7.0.0</tomcat.version>
    </properties>
    
    <dependencies>
      <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>tomcat-servlet-api</artifactId>
        <version>${tomcat.version}</version>
        <scope>provided</scope>
      </dependency>
      <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>tomcat-jsp-api</artifactId>
        <version>${tomcat.version}</version>
        <scope>provided</scope>
      </dependency>
    </dependencies>

### 자유와 편리

2-3 일 삽질했는데 결과로 EL + JSTL 로 부터 자유로워졌습니다. =,=

자주 쓰는 유틸리티 메서드를 JspBase 에 다 박아두면 콜하기 아주 편해집니다.


{:class="go-to-index"}
[Java Spring](index)
