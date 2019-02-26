---
title: Open EntityManager in View Pattern
---

2010-09-30

일반적으로 하이버네이트 세션은 `@Transaction` 안에서만 살아 있습니다.
JPA 식으로 말하면 트랜젝션이 끝나면 엔터티 메니저가 닫혀버립니다.

여기까지는 좋은데 아직 디비에서 끌고 나오지 못한 프락시들이 있다면
이 프락시들을 JSP 에서 참조할 때 데이터를 가지고 오지 못해 에러를 뿌립니다.

그래서 웹 리퀘스트 전체로 엔터티 메니저 생명기간을 확장할 필요가 있는데
스프링에서는 스프링 설정에 아래와 같이 적어줍니다.

    <mvc:interceptors>
      <bean name="openSessionInViewInterceptor"
            class="org.springframework.orm.jpa.support.OpenEntityManagerInViewInterceptor">
        <property name="entityManagerFactory" ref="emf"/>
      </bean>
    </mvc:interceptors>


{:class="go-to-index"}
[Java Spring](index)
