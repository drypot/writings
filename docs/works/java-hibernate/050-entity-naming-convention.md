---
title: Entity Naming Convention
---

2010-09-17

하이버네이트가 자바 클래스명과 필드명을 디비의 테이블명과 컬럼명에 자동으로 매칭시키는데
이 네이밍 변환에 다양한 전략 클래스를 꼽아 사용할 수 있습니다.

### JPA Entity Naming Convention

JPA 스팩이나 나 Hibernate 에는 디비 테이블 이름에 대한 규칙은 따로 마련되어 있지 않습니다.
JPA 스펙 같은 경우 이름 변환을 아예 하지 않거나
디비에서 테이블과 컬럼에 대해 대소문자 구분을 하지 않는 다는 것을 전제로 만들어져 있습니다.
Java Class 이름이 AccessLog 일때 디비 테이블 이름이 ACCESSLOG 라도 아무 문제가 없고
JPA 에서는 사실 이런 케이스 무시를 좀 권장하는 듯한 느낌을 받았습니다.


### ImprovedNamingStrategy

전에 제가 적었던 'Hello, Spring + Hibernate' 라는 글에서는 `ImprovedNamingStrategy` 를 사용합니다.
자바식 명명을 모두 소문자 + 언더라인 들어간 MySql 문화의 이름으로 바꿔줍니다.

    Account class -> account table
    AccountOrder class -> account_order table
    accountId field -> account_id column

해당 하이버네이트 설정은 스프링 설정파일을 통해서 하게 되는데,
스프링 설정파일의 `EntityManagerFactoryBean` 정의하는데 들어가 있습니다.

    <bean id="emf" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
      <property name="jpaProperties">
        <props>
          <prop key="hibernate.ejb.naming_strategy">org.hibernate.cfg.ImprovedNamingStrategy</prop>
        </props>
      </property>
    </bean>

자바식 이름을 디비에 까지 적용하는 것이 어떨까하는 생각이 들 수도 있는데요,
자바식 이름을 자바에서 쓰면 이쁜데,
다른 언어나 다른 도구에서 보면 해괴망칙하게 보입니다. =o=

### MySQL Naming Strategy

MySQL 의 테이블 이름 다루기기는 좀 이상합니다.
컬럼명은 대소문자 구분이 없지만 테이블 명의 경우 운영체제 3 종 마다 이름 관리 규칙이 다릅니다.
특히 개발자들이 많이 사용하는 윈도우의 경우 모든 테이블명을 강제로 소문자로 만들도록
--lower-case-table-names=1 설정을 많이 해서 사용합니다.

테이블 명이 죽어도 소문자 밖에 되질 않으니
MySQL 커뮤니티에서는 테이블 이름에서 단어 구분을 위해 언더라인을 많이 씁니다.
테이블 이름에서 그러니 케이스 구분이 필요 없는 컬럼명에까지 같은 규칙을 사용하고 있고요.
사실 다른 디비 환경들과 좀 다른 문화.

MySQL 을 안 쓰신다면 스프링의 엔터티메니저 설정에서 ImprovedNamingStrategy 등록 설정을 빼버리시고
언더라인 없이 쓰셔도 됩니다.


{:class="go-to-index"}
[Java Hibernate](index)
