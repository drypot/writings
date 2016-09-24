---
title: Beans in JSP
---

2010-10-25

스프링빈을 JSP 에서 편하게 쓰기위한 절차입니다.
목표는 아래처럼 스프링빈인 app 을 JSP EL 에서 아무 생각없이 가져다 쓰는 것입니다.

    <%@page language="java" contentType="text/html; charset=UTF-8"%>
    <div id="site-title">
    <c:out value="${app.siteTitle}"/>
    </div>

일단 아래처럼 스프링빈을 정의합니다. 이름이 "app" 인 싱글턴이 생깁니다.
일반적인 것이니 특별한 것은 없습니다.

    @Component
    public class App  {
      private String siteTitle;
    
      public String getSiteTitle() {
        return siteTitle;
      }
    }

그런후 스프링 설정파일에서 `InternalResourceViewResolver` 등록하실 때
`exposeContextBeansAsAttributes` 속성을 true 로 해주시면 되겠습니다.

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name="exposeContextBeansAsAttributes" value="true" />
      <property name="prefix" value="/WEB-INF/jsp/" />
      <property name="suffix" value=".jsp" />
    </bean>


{:class="go-to-index"}
[Java Spring](index)
