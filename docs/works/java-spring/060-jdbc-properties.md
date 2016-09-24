---
title: JDBC 접속정보 외부로 분리하기 1
---

2010-10-04

프로퍼티 화일은 이런 내용으로, 마음에 드는 아무데나.

    app.jdbc.url=jdbc:mysql://localhost:3306/sleek?characterEncoding=utf8
    app.jdbc.user=root
    app.jdbc.password=
    app.fileWebSiteDir=/Users/drypot/web/sleek/file
    app.luceneDataDir=/Users/drypot/web/sleek/lucene

위에 프로퍼티 파일을 어플리케이션에 전달하기 위한 환경변수는 아래 처럼 

   SLEEK_CONFIG_FILE=/Users/drypot/web/sleek/config/dev.properties

빈들을 세팅하는 스프링 설정화일은 아래와 같이

    <context:property-placeholder location="file:${SLEEK_CONFIG_FILE}" />

    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
      destroy-method="close" init-method="createDataSource">
      <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
      <property name="url" value="${app.jdbc.url}"/>
      <property name="username" value="${app.jdbc.user}"/>
      <property name="password" value="${app.jdbc.password}"/>
      <property name="defaultAutoCommit" value="false"/>
      <property name="initialSize" value="3"/>
      <property name="maxActive" value="16"/>
      <property name="maxIdle" value="5"/>
      <property name="maxWait" value="30000"/>
      <property name="validationQuery" value="SELECT 1"/>
    </bean>

    <bean id="searchService" class="com.drypot.sleek.service.SearchService">
      <property name="luceneDataDir" value="${app.luceneDataDir}"/>
    </bean>


{:class="go-to-index"}
[Java Spring](index)
