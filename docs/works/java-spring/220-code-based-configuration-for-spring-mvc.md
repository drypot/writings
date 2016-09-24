---
title: Code-based Configuration For Spring MVC
---

2011-08-29

Spring 3.1.0.M2 부터 기존에 사용하던 스프링 XML 설정 파일들을 거의 쓰지 않을 수 있게 되었습니다.

<http://blog.springsource.com/2011/06/13/spring-3-1-m2-spring-mvc-enhancements-2/>  
<http://blog.springsource.com/2011/06/10/spring-3-1-m2-configuration-enhancements/>  
<https://github.com/SpringSource/greenhouse>

`persistence.xml` 은 말로는 된다고 하나 코드를 열어 봤더니 아직 삭제할 수가 없고,
`web.xml` 을 전통적인 Servlet Spec 하에서 삭제하려면 Servlet 3.0 스펙이 필요해서 tomcat 7 을 써야합니다.
하지만, 현재 tomcat 7.0.16 + Spring 3.1.0.M2 조합하면 `<form:form>` 태그에서 NPE 가 발생합니다.

마일스톤 릴리즈는 Maven Central 에 없으므로 메이븐 저장소 설정을 해주어야 합니다.

    <repositories>
      <repository>
        <id>org.springframework.maven.milestone</id>
        <name>Spring Maven Milestone Repository</name>
        <url>http://maven.springframework.org/milestone</url>
      </repository>
    </repositories>
    
    <properties>
      <org.springframework.version>3.1.0.M2</org.springframework.version>
      <org.springsecurity.version>3.1.0.M2</org.springsecurity.version>
    </properties>
    
    ...
    
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${org.springframework.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>${org.springframework.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${org.springframework.version}</version>
    </dependency>

`web.xml` 파일에서 Spring Context 로딩 방식이 달라집니다.

`spring.xml` 파일로 설정을 할 때는 `web.xml` 에 다음과 비슷하게 적으셨을 겁니다.

    <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
    <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring/spring-config.xml</param-value>
    </context-param>

Java Annotation 방식으로 하려면 아래와 같은 방식으로 수정합니다.

    <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
    <context-param>
      <param-name>contextClass</param-name>
      <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
    </context-param>
    
    <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>com.drypot.sleek.config</param-value>
    </context-param>

여기서 `contextConfigLocation` 은 `@Configuration` 어노테이션이 붙은 클래스나 클래스가 들어 있는 패키지입니다.

XML 파일을 대신하는 @Configuration 클래스는 다음과 같이 기술됩니다.

    @Configuration
    @ComponentScan({"com.drypot.spring","com.drypot.sleek"})
    @EnableTransactionManagement
    @EnableWebMvc
    public class SpringConfig extends WebMvcConfigurerAdapter {
    ...
    }

`@ComponentScan` 은 기존의 `<context:component-scan base-package=""/>` 역할을 합니다.
`@EnableTransactionManagement` 은 `<tx:annotation-driven />`.
`@EnableWebMvc` + `WebMvcConfigurerAdapter` 은 `<mvc:annotation-driven />` 과 기타등등의 역할을 대신 합니다.

기존에 다음과 같이 `<mvc:interceptors>` 를 사용하셨다면 이것도 바꿀 수 있습니다.

    <mvc:interceptors>
      <bean name="openSessionInViewInterceptor" class="org.springframework.orm.jpa.support.OpenEntityManagerInViewInterceptor">
        <property name="entityManagerFactory" ref="emf" />
      </bean>
      <bean name="lastVisitUpdater" class="com.drypot.sleek.interceptor.LastVisitUpdater"/>
      <bean name="securityInterceptor" class="com.drypot.sleek.interceptor.SecurityInterceptor"/>
    </mvc:interceptors>

를 아래와 같이.

    @Bean
    public OpenEntityManagerInViewInterceptor openEntityManagerInViewInterceptor() {
      OpenEntityManagerInViewInterceptor openEntityManagerInViewInterceptor = new OpenEntityManagerInViewInterceptor();
      openEntityManagerInViewInterceptor.setEntityManagerFactory(emf());
      return openEntityManagerInViewInterceptor;
    }
    
    @Bean
    public LastVisitUpdater lastVisitUpdater() {
      return new LastVisitUpdater();
    }
    
    @Bean 
    public SecurityInterceptor securityInterceptor() {
      return new SecurityInterceptor();
    }
    
    @Override
    public void configureInterceptors(InterceptorConfigurer configurer) {
      configurer.addInterceptor(openEntityManagerInViewInterceptor());
      configurer.addInterceptor(lastVisitUpdater());
      configurer.addInterceptor(securityInterceptor());
    }


{:class="go-to-index"}
[Java Spring](index)
