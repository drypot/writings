---
title: JDBC 접속정보 외부로 분리하기 2
---

2010-11-24
 
항프링 설정을 좀 유연하게 하려다보니 스프링 설정 XML + 환경변수 가지고 좀 무리가 있어서
빈 생성을 자바코드로 하도록 바꾸어 봤습니다.
스프링 XML 이 어쩔 수 없이 필요한 부분이 있기 때문에
2011 년에 스프링 새 버전이 나오기 전까지는 XML 파일을 어쨌든 최소한이라도 써야합니다.

    <?xml version="1.0" encoding="UTF-8"?>
    <beans
      xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:p="http://www.springframework.org/schema/p"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:tx="http://www.springframework.org/schema/tx"
      xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-3.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
        http://www.springframework.org/schema/task
        http://www.springframework.org/schema/task/spring-task-3.0.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">
    
      <context:component-scan base-package="com.drypot.spring,com.drypot.sleek" />
      <tx:annotation-driven />
      <mvc:annotation-driven />
      <mvc:interceptors>
        <bean name="openSessionInViewInterceptor" class="org.springframework.orm.jpa.support.OpenEntityManagerInViewInterceptor">
          <property name="entityManagerFactory" ref="emf" />
        </bean>
        <bean name="lastVisitUpdater" class="com.drypot.sleek.interceptor.LastVisitUpdater"/>
        <bean name="securityInterceptor" class="com.drypot.sleek.interceptor.SecurityInterceptor"/>
      </mvc:interceptors>
    </beans>

뭐 다 빠져서 스프링 XML 파일은 매우 간단해 졌습니다.
저기 빼지 못한 스프링 인프라 세팅 태그는 2011 년까진 써야하는 것들입니다.

스프링 XML 파일에 있던 놈들을 모두 자바 코드로 넣으면 아래와 같이 됩니다.

    package com.drypot.sleek;
    
    import java.util.Properties;
    
    import javax.persistence.EntityManagerFactory;
    import javax.sql.DataSource;
    
    import org.apache.commons.dbcp.BasicDataSource;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.DependsOn;
    import org.springframework.context.support.ReloadableResourceBundleMessageSource;
    import org.springframework.orm.jpa.JpaTransactionManager;
    import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
    import org.springframework.web.multipart.commons.CommonsMultipartResolver;
    import org.springframework.web.servlet.view.InternalResourceViewResolver;
    
    import com.drypot.spring.config.Config;
    
    @Configuration
    @DependsOn("app")
    public class SpringConfig {
    
      @Autowired private App app;
    
      @Bean(initMethod = "createDataSource", destroyMethod = "close")
      public DataSource dataSource() {
        Config config = app.getConfig();
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl(config.getString("dataSource/url"));
        dataSource.setUsername(config.getString("dataSource/user"));
        dataSource.setPassword(config.getString("dataSource/password"));
        dataSource.setDefaultAutoCommit(false);
        dataSource.setInitialSize(3);
        dataSource.setMaxActive(16);
        dataSource.setMaxIdle(5);
        dataSource.setMaxWait(30000);
        dataSource.setValidationQuery("SELECT 1");
        return dataSource;
      }
    
      @Bean
      public EntityManagerFactory emf() {
        LocalContainerEntityManagerFactoryBean factoryBean = new LocalContainerEntityManagerFactoryBean();
        factoryBean.setDataSource(dataSource());
        factoryBean.setPersistenceUnitName("default");
        factoryBean.setJpaProperties(getJpaProperties());
        factoryBean.afterPropertiesSet();
        return factoryBean.getObject();
      }
    
      public Properties getJpaProperties() {
        Properties p = new Properties();
        p.put("hibernate.dialect", "org.hibernate.dialect.MySQL5Dialect");
        p.put("hibernate.cache.provider_class", "org.hibernate.cache.NoCacheProvider");
        p.put("hibernate.show_sql", app.isInDev() ? "true" : "false");
        p.put("hibernate.hbm2ddl.auto", "validate");
    //    p.put("hibernate.ejb.naming_strategy", "org.hibernate.cfg.ImprovedNamingStrategy");
    //    p.put("hibernate.cache.use_second_level_cache", "true");
    //    p.put("hibernate.cache.use_query_cache", "true");
        return p;
      }
    
      @Bean
      public JpaTransactionManager transactionManager() {
        JpaTransactionManager tm = new JpaTransactionManager();
        tm.setEntityManagerFactory(emf());
        return tm;
      }
    
      @Bean
      public InternalResourceViewResolver internalResoureViewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/jsp/");
        resolver.setSuffix(".jsp");
        return resolver;
      }
    
      @Bean
      public CommonsMultipartResolver multipartResolver() {
        CommonsMultipartResolver mr = new CommonsMultipartResolver();
        mr.setMaxUploadSize(536870912L); // 512MB
        mr.setDefaultEncoding("UTF-8");
        return mr;
      }
    
      @Bean
      public ReloadableResourceBundleMessageSource messageSource() {
        ReloadableResourceBundleMessageSource source = new ReloadableResourceBundleMessageSource();
        source.setBasename("classpath:messages/messages");
        source.setDefaultEncoding("UTF-8");
        return source;
      }
    
    }


{:class="go-to-index"}
[Java Spring](index)
