---
title: Hello, Spring + Hibernate
---

2010-09-28

### 프로젝트 큰 그림

토비 스프링3 책에 나온 모델을 따라서 Spring + Hibernate 판 Hello 를 만들어 봤습니다.
로직 흐름은 책에서 처럼 Controller -> Service -> Dao 가 됩니다.
그리고 각 섹션간 데이터 전달을 위해 Domain Object 들이 사용됩니다.

도메인 오브젝트를 깔끔하게 만들어 두면
웹 리퀘스트 인자들을 스프링이 도메인 오브젝트에 통으로 담아서 주니 필드가 많을 때 필드 별로 값을 일일이 받지 않아도 됩니다.
섹션에서 섹션으로 데이터를 전달할 때 도메인 오브젝트 하나만 던지면 됩니다.
자바 메서드에 인자를 줄줄이 나열하는 일이 사라집니다.
도메인 오브젝트를 Dao 에서 까지 사용합니다.
도메인 오브젝트를 하이버네이트에 던지면 그대로 디비에 집어 넣습니다.

물론 이런 이상적인 상황은 실무에서는 별로 없습니다.
도메인 오브젝트의 값을 필터링하거나 조작할 필요가 있습니다.
웹 프로토콜에 관련된 작업이라면 컨트롤러에서, 비즈니스 로직에 관련된 작업이라면 서비스 섹션에서 해주면 됩니다.
도메인 오브젝트나 Dao 계층에 비즈니스 로직이 들어가지 않습니다.

어디까지 컨트롤러에서 하고, 어디까지 서비스 섹션에서 하는가.
이건 같은 로직을 웹 사이트와 모바일 사이트에 같이 적용한다고 상상해 보면 됩니다.
다양한 출력에 동일하게 사용되는 로직이라면 서비스 섹션에,
모바일 특화 작업이라면 모바일용 컨트롤러에,
일반 웹 작업이라면 웹 컨트롤러에 담으면 됩니다.


### Domain Object

    package net.directmedia.todaysfortune.domain;
    import ...
    
    @Entity
    public class Account {
    
      @Id
      @GeneratedValue
      private int id;
    
      private String name;
    
      @Column(columnDefinition = "char(40)")
      private String udid;
    
      @DateTimeFormat(pattern = "yyyyMMdd")
      @Type(type = "org.joda.time.contrib.hibernate.PersistentLocalDate")
      private LocalDate birthDate;
    
      private boolean birthDateSolar;
    
      private int popularity;
    
      // getters, setters
    
    }

스프링 컨트롤러에서 부터 하이버네이트 DAO 까지 동일하게 사용되는 도메인 클래스 예입니다.
특징은 클래스 머리에 `@Entity` 가 붙어 있습니다.
`@Entity` 가 붙은 클래스는 하이버네이트에서 자동으로 인지하고 디비 테이블과 매핑할 준비를 합니다.


프라이머리 키가될 `id` 필드에 `@Id` 어노테이션이 붙어 있습니다.
MySql 에서 자동으로 키젠을 할 것이므로 `@GeneratedValue` 도 붙어 있습니다.

평범한 필드들은 자동으로 디비 테이블 컬럼에 매핑이 됩니다.
`name`, `popularity` 등.

특별한 경우에 `@Column` 어노테이션으로 추가 정보를 제공합니다.
자바의 `String` 타입 필드는 기본저으로 `varchar` 로 매핑되기 때문에
`char` 타입 컬럼에 매핑하려면 에러가 납니다.
`udid` 경우 `char(40)` 으로 매핑하기 위해 추가 정보를 붙였습니다.

`@Entity` 클래스의 경우 기본적으로 모든 필드를 테이블의 컬럼과 연관짓습니다.
피치못하게 예외를 두어야 할 경우에 사용하는 어노테이션들이 있습니다.

`birthDate` 같은 경우 타입이 `LocalDate` 인데 이는 자바 기본 클래스 타입이 아니고 joda time 라이브러리에 들어있는 것입니다.
자바 `Date` 클래스는 거의 모든 메서드 들이 deprecated 되어서 사용하기가 껄끄럽고 새로 추가된 `Calendar` 클래스도 문제가 많습니다.
그래서 joda time 라이브러리를 많이 사용하는데
이는 자바 기본 클래스가 아니기 때문에 하이버네이트에서 테이블에 저장할 방법을 가지고 있지 않습니다.
그래서  joda time 관련 클래스의 저장법을 하이버네이트에 지정하기 위해 조금 긴 어노테이션이 필요합니다.

`@DateTimeFormat(pattern = "yyyyMMdd")` 이 어노테이션은 스프링에서 사용합니다.
인자로 넘어온 스트링을 LocalDate 타입으로 변환하는 포멧을 지정한 것입니다.

엔터티 정의가 좀 복잡하지만 이것으로 일의 절반 이상은 끝났습니다.
이렇게 한번 도메인 오브젝트를 잘 정의해 두면 스프링과 하이버네이트에서 반복적으로 사용될 때 반복작업이 대폭 줄어듭니다.

도메인 오브젝트에는 아주 간단한 로직 외에는 담지 않습니다.
왜냐면 이 오브젝트는 웹에서 부터 디비 섹션까지 두루두루 사용되기 때문에
꼭 중립적인 기능만 넣어 둡니다.


### Controller Object

    package net.directmedia.todaysfortune.controller;
    import ...
    
    @Controller
    public class AccountController {
    
      @Autowired
      private AccountService accountService;
    
      @RequestMapping(value = "/account/create", method = RequestMethod.GET)
      @ResponseBody
      public String create(Account account, BindingResult result) {
        try {
          accountService.create(account);
          return String.valueOf(account.getId());
        } catch(PersistenceException e) {
          return "-1";
        }
      }
    
      @RequestMapping(value = "/account/get", method = RequestMethod.GET)
      @ResponseBody
      public String get(String udid) {
        try {
          Account account = accountService.getByUdid(udid);
          StringBuilder buf = new StringBuilder(512);
          buf.append( ... );
          return buf.toString();
        } catch(NoResultException e) {
          return "-1";
        }
      }
    }

웹 리퀘스트를 받는 컨트롤러부입니다.
웹 리퀘스트에서 인자를 받고 서비스 오브젝트에 넘기는 일까지만 합니다.
서비스 오브젝트에서 결과를 돌려주면 내용을 웹에 적당하게 출력합니다.
비즈니스 로직은 절대 담지 말고 디비 관련 로직도 없어야 합니다.

기타 나머지 Spring MVC 규약에 따르는 어노테이션들이 붙어 있습니다.
`@Autowired` 선언되어 있는 필드는 스프링에서 자동으로 오브젝트를 연결해 줍니다.


### Service Object

    package net.directmedia.todaysfortune.service;
    import ...
    
    @Component
    public class AccountService {
    
      @Autowired
      private AccountDao accountDao;
    
      @Transactional
      public void create(Account account) {
        accountDao.create(account);
      }
    
      @Transactional
      public void update(Account account) {
        accountDao.update(account);
      }
    
      @Transactional
      public Account getByUdid(String udid) {
        return accountDao.getByUdid(udid);
      }
    
      ...
    }

서비스 오브젝트 클래스입니다.
아직 비즈니스 로직이 들어차기 전이라 내용이 거의 없습니다.
도메인 오브젝트를 받아서 DAO 로 넘기는 작업만 하고 있습니다.
DAO 오브젝트 필드는 역시나 스프링으로부터 자동으로 생성 받습니다.

스프링 & 하이버네이트 프레임웍 위에서는 어디에도 Data Connection 을 열고 닫는 코드가 없습니다.
대신 @Transactional 어노테이션을 사용합니다.
실제로 벌어지는 작동의 양상은 좀 복잡하지만,
@Transactional 의 메서드에 진입하는 시점에 컨넥션이 열리고, 나가는 시점에 닫힌다고 보셔도 되겠습니다.
보통 서비스 섹션 메서드들에 @Transactional 을 많이 붙입니다.
한 서비스에서 여러 Dao 오퍼레이션을 할 수 있는데,
같은 트랜젝션 내에서는 같은 컨넥션과 오브젝트 풀을 공유하게 됩니다.


### DAO Object

    package net.directmedia.todaysfortune.dao;
    import ...
    
    @Component
    public class AccountDao {
    
      @PersistenceContext
      EntityManager em;
    
      public void create(Account account) {
        em.persist(account);
      }
    
      public Account get(int accountId) {
        return em.find(Account.class,  accountId);
      }
    
      public Account getByUdid(String udid) {
        return (Account) em.createQuery("select a from Account a where a.udid = ?")
            .setParameter(1, udid)
            .getSingleResult();
      }
    
      public void update(Account account) {
        em.merge(account);
      }
    }

하이버네이트의 DAO 오브젝트 부분입니다.
좀 썰렁하지만, 이걸로 디비 CRUD 오퍼레이션이 모두 가능합니다.
JDBC 같았으면 `insert into account(id, name ,,,, ) values ( ?, ?, ,? ,,,)` 나
`select` 후에 필드 매핑하는 노가다 코드가 나왔을 것이나
하이버네이트에서는 오브젝트 단위로 데이터를 넣고 뽑기 때문에 필드 매핑 스크립트가 나타나지 않습니다.

`em.persist(account)` 진짜 이걸로 오브젝트가 MySql account 테이블에 들어갑니다.

기능은 훨씬 많이 하지만 `EntityManager` 엔터티메니저는 하이버네이트의 JDBC Connection 이라고 보셔도 되겠습니다.
특이한 것은 `@Autowired` 대신에 `@PersistenceContext` 어노테이션을 사용합니다.
기본적으로 모든 스프링 오브젝트는 싱글턴입니다.
DAO 오브젝트들도 마찬가지로 시스템에 딱 하나만 생성되고 모든 스레드가 공유합니다.
그런데 보통 컨넥션은 멀티스레드 환경에서 공유가 안 됩니다.
이 문제를 해결하기 위해 `@PersistenceContext` 가 등장하는데,
보기에는 싱글턴을 연결한 것 같지만, 실제로는 컨섹션을 돌려주는 프락시가 연결되서
각 스레드의 트랜젝션 별로 각각의 엔터티메니저를 돌려줍니다.


### XML XML XML

스프링은 자체 XML 설정 방식을 가지고 있습니다.
스프링 책 보시면서 이제 눈에 익으셨을 것 같습니다.

하이버네이트도 자체 XML 설정 방식을 가지고 있습니다.
게다가 과거에는 자바 클래스와 테이블 컬럼 매핑도 일일이 XML 을 가지고 했었습니다.

더 문제는, JPA 스팩도 자체 XML 설정 방식을 가지고 있습니다.

네, 3 가지 시스템 설정을 연결하는데 며칠 짜증이 밀려왔었습니다. =,=

다양한 방식이 있을 수 있는데, 결론은,
가능하면 스프링 설정에 다른 설정을 모두 포함시키는 겁니다.
그러면 하이버네이트 설정 파일은 만들지 않아도 됩니다.
JPA 설정까지 스프링안에 가두면 좋겠는데, 그것 까지는 안 되더군요.
간단한  JPA 용 파일을 하나 빼두어야 합니다.

  
### JPA XML

    <?xml version="1.0" encoding="UTF-8"?>
    <persistence
      xmlns="http://java.sun.com/xml/ns/persistence"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
      version="2.0">
    
      <persistence-unit name="default" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.ejb.HibernatePersistence</provider>
      </persistence-unit>
    
    </persistence>

JPA 설정파일입니다.
보통 JPA 문서에는 되게 길게 나와 있는데 필요한 정보를 스프링 설정으로 모두 옮겨서 매우 간출해졌습니다.
딱 하나 저기 보이는 건, JPA 를 쓰긴 쓰는데 그것의 구현은 실제 하이버네이트다라는 것입니다.
이클립스 링크나 OpenJPA 를 쓴다면 프로바이더가 달라지겠지요.

이 파일은 반드시 클래스패스 내의 `META-INF/persistence.xml` 로 있어야 합니다.
메이븐 프로젝트 기준으로 본다면, `src/main/resources/META-INF/persistence.xml` 입니다.
이 위치에 그대로 복사해 넣으시면 되겠습니다.


### Spring XML

이 글을 편집하고 있는 시점에 Spring 설정을 XML 대신 Java 코드로 할 수 있는 방법이 소개되고 있으나
Spring 3.1 stable 버전이 아직 릴리즈되지 않았으므로 일단은 XML 설정법으로 설명을 하겠습니다.

    <tx:annotation-driven/>
    
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
      <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
      <property name="url" value="jdbc:mysql://localhost/todays_fortune"/>
      <property name="username" value="root"/>
      <property name="password" value=""/>
      <property name="defaultAutoCommit" value="true"/>
      <property name="initialSize" value="5"/>
      <property name="maxActive" value="30"/>
      <property name="maxIdle" value="5"/>
      <property name="maxWait" value="30000"/>
      <property name="validationQuery" value="SELECT 1"/>
    </bean>
    
    <bean id="emf" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
      <property name="dataSource" ref="dataSource"/>
      <property name="jpaProperties">
        <props>
          <prop key="hibernate.dialect">org.hibernate.dialect.MySQLMyISAMDialect</prop>
          <prop key="hibernate.cache.provider_class">org.hibernate.cache.NoCacheProvider</prop>
          <prop key="hibernate.show_sql">true</prop>
          <prop key="hibernate.hbm2ddl.auto">validate</prop>
          <prop key="hibernate.ejb.naming_strategy">org.hibernate.cfg.ImprovedNamingStrategy</prop>
        </props>
      </property>
    </bean>
    
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
      <property name="entityManagerFactory" ref="emf"/>
    </bean>


스프링 설정입니다.
스프링 설정 파일에 위에 선언들 군데군데 넣어주시면 됩니다.

`<tx:annotation-driven/>` 는 `@Transactional` 을  쓰겠다는 것입니다.
꼭 있어야 합니다.

`org.apache.commons.dbcp.BasicDataSource` 오브젝트는 스프링풀 지원하는 데이서소스 선언입니다.
뭘쓰던 어쨌든 데이터 소스는 필요하니까요.

`LocalContainerEntityManagerFactoryBean` 이 JPA 엔터티메니저 관련 부분을 초기화 해주는 스프링 서비스입니다.
스프링에서 모두 노다가를 대신 해주어서 저렇게 가볍게 오브젝트 하나 만들어 주면 JPA 초기화가 됩니다.

엔터티메니저 초기화부에 하이버네이트 특화된 설정들이 보이는데,
`hibernate.dialect` 는 실제로 MySql 쓰겠으니 MySql 용 SQL 을 생성하라는 말이고,
`hibernate.show_sql` 을 `true` 로 해두면 개발시 실행되는 실제 SQL 을 콘솔에 계속 뿌려줍니다. 서비스 서버에서는 `false` 로.
`hibernate.hbm2ddl.auto` 를 `validate` 로 하면, 처음 기동시 디비 스키마와 자바 클래스를 비교해서 매칭 안되면 뻣습니다.
`hibernate.ejb.naming_strategy` 설정은 자바방식 네이밍을 MySql 방식 네이밍으로 자동변환해 줍니다.

`JpaTransactionManager` 오브젝트도 선언하는데
스프링의 트랜젝션 시스템을 JPA 의 트랜젝션 시스템과 연동한다는 말입니다.
스프링은 자체 트랜젝션 보조 서비스를 갖추고 있지만, 실제로 동작하는 트랜젝션 서비스를 가지고 있진 않습니다.
우리는 현재 JPA API 를 쓰기 때문에 JPA 가 제공하는 트랜젝션 서비스에 스프링 트랜잭션을 연결하면 됩니다.
결과로는 우리 코드에 보이는 스프링 어노테이션들 즉, `@Transactional` 어노테이션으로 동작하는 
스프링 트랜잭션 서비스들이 모두 하이버네이트의 관련 기능들과 들러붙습니다.

### Maven Dependency

아래 버전 조합은 2010 년 9 월 경 맞춘 것입니다.

    <properties>
      <maven.test.skip>true</maven.test.skip>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <org.springframework.version>3.0.4.RELEASE</org.springframework.version>
      <org.hibernate.version>3.5.5-Final</org.hibernate.version>
    </properties>
    
    <dependencies>
    <!-- 스프링 관련 -->
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
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${org.springframework.version}</version>
      </dependency>
      <!-- 하이버네이트 엔터티메니저 패키지 링크걸면 어노테이션, 코어 다 따라옵니다. -->
      <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>${org.hibernate.version}</version>
      </dependency>
      <!-- JPA2 API, 전통적으로 JSR API 는 메이븐 리포지터리를 무시하기 때문에.. 하이버네이트에서 제공하는 것으로 =,= -->
      <dependency>
        <groupId>org.hibernate.javax.persistence</groupId>
        <artifactId>hibernate-jpa-2.0-api</artifactId>
        <version>1.0.0.Final</version>
      </dependency>
      <!-- 하이버네이트는 CGLIB 프로젝트가 죽어서 자바시스트로 이사갔는데, 스프링은 아직 이걸 씁니다 --> 
      <dependency>
        <groupId>cglib</groupId>
        <artifactId>cglib</artifactId>
        <version>2.2</version>
      </dependency>
      <!-- 조다 타임 관련 -->
      <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
        <version>1.6</version>
      </dependency>
      <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time-hibernate</artifactId>
        <version>1.2</version>
        <!-- 그냥 링크걸면 하이버네이트 구버전이 따라오면서 쫑나니 아래 처럼 익스클루드 -->
        <exclusions>
          <exclusion>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate</artifactId>
          </exclusion>
        </exclusions>
      </dependency>
      <!-- 스프링에서 파일 업로드 기능 구현에 사용 -->
      <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.2.1</version>
      </dependency>
      <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>1.4</version>
      </dependency>
      <!-- Servlet API, 정식 7.0 API 릴리즈를 쓰려면 Java EE 다 받아야 하니 걍 톰켓에 들어 있는거 간단히 씁시다 --> 
      <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>tomcat-servlet-api</artifactId>
        <version>7.0.0</version>
        <scope>provided</scope>
      </dependency>
      <!-- 디비 컨넥션 풀 라이브러리 -->
      <dependency>
        <groupId>commons-dbcp</groupId>
        <artifactId>commons-dbcp</artifactId>
        <version>1.4</version>
      </dependency>
      <!-- 보시다시피 MySql 드라이버 -->
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.13</version>
      </dependency>
      <!-- 로깅 라이브러리 -->
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.5.8</version>
      </dependency>
    
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.8.1</version>
        <scope>test</scope>
      </dependency>
    
    </dependencies>


### 진짜 길군요

Java 개발 환경의 현실입니다. 누굴탓할까요.
결과는 간단한데 Spring / JPA / Hibernate 연결하느라 머리에 쥐나는 줄 알았습니다.
그나마 바탕에 이렇게 한번 깔아 두면 그 위에 하이버네이트 동작시키고 비즈니스 로직 구현하는 것은 조금 수월해 집니다.


{:class="go-to-index"}
[Java Hibernate](index)
