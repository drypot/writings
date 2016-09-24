---
title: Hello, Spring + Eclipse + Maven
---

2010-08-24

스프링을 맛보기 할 수 있는 초 짧은(?) 절차다.
Eclipse 3.5 기준으로 작성된 글이므로 이후 버전에선 차이가 있을 수 있다.

### Maven

문제는 스프링보다 Maven 이다.
Maven 은 프로젝트 관리시스템인데 이클립스와는 별도의 생태계를 가지고 있다.
미래는 분명 Maven 인데, 아직 불편한 점들도 많아서 기존 기술들을 버리고 완전히 갈아타기는 무리가 있다.
이클립스와 현명하게 잘 섞어 사용해야 한다.

이클립스에 Maven 플러그인을 설치하지 않았다면, 아래 페이지 내용대로 m2e 를 설치한다.  
http://eclipse.org/m2e/

### 프로젝트 생성

이클립스에서 File -> New -> Project ... -> Maven Project -> 적당히 채운다. 

다음, Maven Archetype 선택하는 곳에서 먼가 많이 나오는데, 아래 아키타입을 선택해 준다.  
group id : org.apache.maven.archetypes  
artiface id : maven-archetype-quckstart

이클립스 기본 프로젝트 타입이었다면 Net Project 화면에서 바로 선택가능했겠지만
Maven 프로젝트이기 때문에 Maven 선택을 하고
Maven 의 프로젝트 타입인 아키타입을 이렇게 별도로 찾아주어야 한다.

다음, 이제 내 프로젝트 정보입력화면이 나오는데
group id 에는 역순 도메인 이름 (예:net.directmedia),
artifact id 에는 test-spring 정도 입력하고 완료한다.

Maven 이 처음이면 이 과정이 황당한데 곧 익숙해진다.

### pom.xml

    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
    
      <groupId>net.directmedia</groupId>
      <artifactId>test-spring</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>jar</packaging>
    
      <name>test-spring</name>
    
      <properties>
        <maven.test.skip>true</maven.test.skip>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <org.springframework.version>3.0.3.RELEASE</org.springframework.version>
      </properties>
    
      <dependencies>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>3.8.1</version>
          <scope>test</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-core</artifactId>
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
      </dependencies>
    
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
              <source>1.6</source>
              <target>1.6</target>
            </configuration>
          </plugin>
        </plugins>
      </build>
      
    </project>
  
pom.xml 은 maven 설정파일이다.
공 프로젝트에 pom.xml 이 만들어져 있을 것이다.
pom.xml 을 위 내용으로 바꾼다.
스프링은 꽤 많은 라이브러리를 사용하는데 일일이 다운 받으려면 너무 귀찮기 때문에
Maven 의 디펜던시 설정으로 자동으로 받게 만들려는 목적이다.
스프링 코드를 작성하는데 필요한 기본 세팅들이 되어 있으니
한번 만들어 두면 당분간은 저정도 세팅으로 계속 때울 수 있다.

그리고 중요한 것 한가지.
Maven 가족의 컴파일러 플러그인이 기본으로 Java 1.4 를 쓰도록 되어 있어서
위에 pom.xml 내용을 보면 중간에 Java 1.6 을 쓰도록 수정하는 부분이 들어있다.
그런데, Maven 과 이클립스 프로젝트 설정이 연결이 안 되어서
이클립스 설정에는 1.4 로 그대로 남아있을 수 있으므로
이클립스 설정을 업데이트 해주어야 한다.
방법은 이클립스 프로젝트 익스플로러에서 프로젝트 이름 선택한 후 -> 마우스 오른쪽 버튼 ->
메뉴 많이 나오는데 한참 찾아보시면 Maven 항목이 생겨 있다 ->
거기서 Update Project Configuration 한번 눌러준다.
그러면 이클립스 프로젝트 설정에 Java Compiler 가 1.6 으로 된다.


### DataSource Object

    package com.drypot.test.spring;
    
    import org.springframework.stereotype.Component;
    
    @Component
    public class DataObject {
      public String getValue() {
        return "My Precious";
      }
    }


초간단 데이터 소스 오브젝트다.
`@Component` 어노테이션이 붙어있는데, 스프링 스캐너에 자동으로 인식시키기 위해 붙였다.
스프링 오브젝트는 스프링이 알아서 만들고 서로들 연결시켜 준다.
이렇게 해주면 우리가 오브젝트를 생성할 일이 없어진다.

여기서 잠깐, `new` 로 만들고 대입하면 될 껄 왜 스프링한테 맡길까?
첫째, DataSource 같은 오브젝트는 꽤 많은 오브젝트에서 사용되는데
이제 이런 것은 일일이 대입해주지 않아도 된다. 스프링이 알아서 한다.
둘째, 이렇게 클래스 밖으로 오브젝트 연결 기능을 뽑으면 클래스 디자인이 몹시 좋아진다.
믿어달라. 한참 나중에 느낄 수 있다.

### Service Object

    package com.drypot.test.spring;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Component;
    
    @Component
    public class Service {

      @Autowired DataObject dataObject;

      public void processIt() {
        System.out.println(dataObject.getValue());
      }
    }

데이터를 소모하고 우리의 요청을 들어줄 서비스 오브젝트다.
이놈도 스프링이 관리하는 스프링 오브젝트가 될 대상이므로 `@Component` 로 어노테이션 하였다.
또 중간에 `@Autowired` 라는 어노테이션이 보이는데, 스프링보러 오브젝트 연결하라는 의미다.
우리가 직접하지 않는다.
다시 적지만 사실 이렇게 대입문을 제 3 자에게 위임하는 것은 작은 코딩 편의성 때문이라기 보다
클래스 디자인이 꽤 좋아지는 효과가 있기 때문이다.


### Main

    package com.drypot.test.spring;
    
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class App 
    {
      public static void main( String[] args )
      {
        AnnotationConfigApplicationContext appContext =
          new AnnotationConfigApplicationContext("com.drypot.test.spring");
        Service service = appContext.getBean("service", Service.class);
        service.processIt();
      }
    }
  
메인 클래스이다. 데이터, 서비스가 준비되었으니 이것들을 써 본다.

먼저 스프링 오브젝트를 총괄하는 스프링 컨텍스트 `appContext` 를 만들어 준다.
`Anno...` 클래스 이름이 무지 긴데 무시하고 걍 Copy & Paste 한다.
한번 만들면 끝나기 때문에 프로젝트에서 나중에 볼 일도 없다.
`Anno...` 오브젝트 생성자에 던지는 인자는 스프링이 관리할 클래스를 스캐닝할 대상 패키지명이다.
스캐너는 `@Component` 어노테이션이 붙은 클래스들을 찾고 알아서 오브젝트들을 만들고 연결해 준다.

스프링이 오브젝트들을 싹 준비해 놨을 것이므로 레퍼런스를 얻어 본다.  
`appContext.getBean("service", Service.class);`

오브젝트 레퍼런스가 얻어졌으므로 호출한다.  
`service.processIt();`

실행한다.  
`Shift-Alt-X + J`

결과가 출력되었다.

    My Precious


### 왜 이 짓을 하는가

Hello 치고는 무척 길었는데 직접 오브젝트 생성과 연결을 하지 않고 왜 이 짓을 하는지 가슴에 잘 와닿지 않을 것이다.
하지만, 프로젝트가 복잡해지고 요구사항이 계속 바뀌는 환경에서는 장점이 많다.
이 장점을 얻기 위해서는 스프링을 사용하는 것 뿐만 아니라  어플리케이션 비즈니스 클래스들의 설계도 잘 만들어야할 필요가 있다.


{:class="go-to-index"}
[Java Spring](index)
