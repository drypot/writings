---
title: Hello, Spring MVC + Eclipse + Maven
---

2010-09-01

### 프로젝트 생성

디비 관련해서 같이 적으려고 했는데 이쪽은 다음 주나 정리가 될 것 같습니다.
그래서 오늘은 일단 초짧은 스프링 MVC 프로젝트 준비하는 절차 적어보겠습니다.
이클립스 갈릴레오 버전에서 진행했습니다.
이클립스 헬리오스에서는 군데 군데 살짝 좀 달라지긴 합니다.
(다중 프로젝트 연결할 때 좀 다릅니다.)

File -> New -> Project -> Maven Project 갑니다.
메이븐 아키타입 선택화면에서 아티펙트 maven-archetype-webapp 선택합니다.
이클립스 네이티브한 Dynamic Web App 프로젝트로 부터 진행해도 되는데
Maven 스타일 구조가 안 나오므로 애초에 maven 프로젝트로 시작합니다.

아키타입 인자 지정화면에서
group id : net.directmedia (또는 원하시는 역순 도메인 이름),
artifact id : test-spring-mvc,
package: net.directmedia.mvcdemo (또는 원하시는 패키지명) 을 지정합니다.

### pom.xml

    <project
      xmlns="http://maven.apache.org/POM/4.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    
      <modelVersion>4.0.0</modelVersion>
    
      <groupId>net.directmedia</groupId>
      <artifactId>test-spring-mvc</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
    
      <name>test-spring-mvc</name>
    
      <properties>
        <maven.test.skip>true</maven.test.skip>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <org.springframework.version>3.0.4.RELEASE</org.springframework.version>
      </properties>
    
      <dependencies>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>3.8.1</version>
          <scope>test</scope>
        </dependency>
        <dependency>
          <groupId>joda-time</groupId>
          <artifactId>joda-time</artifactId>
          <version>1.6</version>
        </dependency>
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
        <finalName>test-spring-mvc</finalName>
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

일단 초기 pom.xml 파일 내용을 위 내용으로 교체합니다.
스프링 관련 디펜던시가 추가 되었고 자바 기본 Date / Calendar 클래스가 너무 후져서 joda-time 추가했습니다.
joda-time 은 스프링에서도 기본적으로 여기 저기서 지원해 주니 쓰시는 것을 추천합니다.
스프링에서 파일 업로드 처리할 때 커먼즈 파일 업로드 관련 라이브러리를 불러다 쓰니 꼭 추가해줘야 합니다.
메이븐 기본 JDK 버전이 1.4 로 세팅되어 있어서 1.6 으로 세팅하는 부분도 들어 있습니다.

위 내용을 저장하시고 프로젝트 -> 오른쪽 마우스 버튼 -> Maven -> Update Project Configuration 클릭해서
Maven 이클립스간 프로젝트 정보를 싱크합니다.

### Make Java Src Folder

자바 소스 폴더를 만듭니다.
프로젝트를 진행하시다 보면 자바 로직은 별도 메이븐 프로젝트로 뽑고
웹 프로젝트에서는 JSP / CSS 등만 관리하시게 될 터인데 간단 예제이므로 한 프로젝트에 다 몰아넣겠습니다.

프로젝트 -> 오른쪽 마우스 버튼 -> Properties -> Java Build Path -> Source Tab.
여기서 Add Folder 하셔서 src/main/java 디렉토리를 만듭니다.
디렉토리에 채크 표시가 자동으로 되었을 것이므로 OK.
프로젝트 익스플로러 -> Java Resources 밑에 src/main/java 가 등록되었을 것입니다.

### Check Eclipse Perspective

이클립스의 현재 퍼스펙티브에 따라 메뉴나 프로젝트 정보 위치 구성이 다를 수 있습니다.
현재 저는 JavaEE 퍼스펙티브에서 설명하고 있습니다.
퍼스펙티브 변경은 이클립스 좌상단 귀퉁이에 아이콘들 가지고 할 수 있습니다.

### Controller Object
    
    package net.directmedia.mvcdemo.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.ResponseBody;
    
    @Controller
    public class DemoController {
      @RequestMapping(value = "/hello", method = RequestMethod.GET)
      @ResponseBody
      public String hello(@RequestParam String name) {
        return "Hello " + name;
      }
    }

첫 자바 코드를 추가해 봅니다.
좋아하시는 방식으로 New -> Class 파일 하시고
package: net.directmedia.mvcdemo.controller,
class: DemoController
정도로 패키지와 파일을 생성하신 후에 위 내용을 넣어줍니다.

스프링 MVC 서블릿이 /hello URL 로 들어오는 리퀘스트를 컨트롤러의 해당 메서드로 보내줍니다.
매핑 정보는 어노테이션으로 기술되어 있습니다.

메서드의 인자로 URL Query Parameter 를 받을 수 있습니다.
URL 이 "/hello?name=drypot" 이었다면 name 인자로 drypot 이 전달됩니다.

### web.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <web-app
      xmlns="http://java.sun.com/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
      version="2.5">
    
      <servlet>
        <servlet-name>spring</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
      </servlet>
    
      <servlet-mapping>
        <servlet-name>spring</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping>
    
    </web-app>

자바 클래스가 있다고 바로 실행될 수는 없지요.
웹 컨테이너, 스프링 컨테이너, 단계 단계 준비를 합니다.
일단 웹 컨테이너를 위한 web.xml 부터.

프로젝트 익스플로러 -> Web Resource -> WEB-INF -> web.xml 파일이 만들어져 있을 터인데
이 파일의 내용을 위 내용으로 치환합니다.
Spring MVC 를 쓰기 위해서는 Spring DispatcherServlet 을 등록해줘야합니다.
그래야 Tomcat 에서 리퀘스트를 스프링에 전달할 수 있습니다.
수정하시고 저장.

### spring-servlet.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:p="http://www.springframework.org/schema/p"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-3.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">
    
      <context:component-scan base-package="net.directmedia.mvcdemo" />
      <mvc:annotation-driven/>
      <!--<mvc:default-servlet-handler/>-->
    
      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
      </bean>
    
      <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="16777216" /> <!-- 16 MB -->
        <property name="defaultEncoding" value="UTF-8" />
      </bean>
    
    </beans>

먼가 또 가득 나왔습니다.
Tomcat 설정이 끝났으니 Spring 설정을 해줘야합니다.
먼가 복잡하지만 다행히도 이걸다 외울 필요는 없습니다.
저도 스프링 사이트에서 가져다 붙였습니다.
그리고 앞으로 별로 수정할 필요가 없거나 조금씩만 Copy & Paste 하면 됩니다.
위 파일을 `WEB-INF/spring-servlet.xml` 로 저장해줍니다.
저장해 놓으면 스프링 DispatchServlet 이 알아서 읽고 스프링 컨텍스트를 초기화 합니다.

각 행별 의미는 다음과 같습니다.

    <context:component-scan base-package="net.directmedia.mvcdemo" />

스프링에게 해당 패키지를 스캐닝하라는 의미입니다.
무엇을 스캐닝 하냐면 위에서 어노테이션으로 설정한 `@Controller` 나 `@RequestParam` 같은 메타 정보를 스케닝하라는 말입니다.
이 문장 설정이 있어야 스프링과 우리가 만든 컨트롤러 코드가 연결됩니다.

    <mvc:annotation-driven/>

스프링은 모듈별로 옵션이 많은데 MVC 구현 방식도 그렇습니다.
스프링에게 우리가 사용할 MVC 가 어노테이션 방식이니 그에 따라 Spring MVC 인프라 내부 설정을 하라는 말입니다.

기본으로 설정되는 Spring MVC 인프라 외에 추가로 오브젝트를 등록해서 설정을 변경할 수 있습니다.
그중에 편리한 것이 `InternalResourceViewResolver` 인데
컨트롤러와 JSP 페이지를 연결할 때 뷰 파일 이름을 좀 짧게 적을 수 있게 해줍니다.
`InternalResourceViewResolver` 기본설정 대로라면 `/WEB-INF/jsp/somepage.jsp` 라고 길게 적어주어야 하는데,
프리픽스/서픽스 지정해 놓으면 `somepage` 라는 간단한 스트링으로 뷰 페이지 이름을 지정할 수 있습니다.

`CommonsMultipartResolver` 는 첨부파일 처리 모듈을 위해 등록했습니다.
이게 없으면 첨부파일 처리를 못합니다.

### Eclipse Project Setting

대략 코드나 설정파일 준비는 마무리 되었습니다.
톰켓에 올리기 전에 프로젝트 설정을 좀 마무리 해줍니다.
프로젝트 프로퍼티에서 Web Project Settings 에 context root 를 "/" 로 변경합니다.

### Eclipse Tomcat Plugin Setting

이클립스에 Tomcat 을 꼽아 봅시다.
메뉴에서 Window -> Show View -> Servers 로 Servers 판낼을 켭니다.
화면 하단이나 오른쪽이나에 판낼이 나왔을 겁니다.
판낼에서 오른쪽 마우스 버튼 -> New -> Server.
Define a New Server 다이얼로그 뜹니다.
여기서 Apache / Tomcat 6.0 선택해주세요.
다음 톰켓 설치 디렉토리를 뭍는데 본인이 깔아 놓은 톰켓 6.0 디렉토리를 지정합니다.
새로 만든 톰켓 서버 설정에 추가할 프로젝트 선택 화면이 나오는데 방금 만든 test-spring-mvc 를 추가해 줍니다.
Finish.

Servers 판낼에 새로 만든 톰켓 설정이 등록이 되었을 겁니다.
이대로 쓰면 좀 불편하니 몇 군데 고쳐봅니다.
Servers 판낼의 서버 설정 위에서 오른쪽 마우스 버튼 -> Open.
서버 설정 오버뷰 화면이 나옵니다.
여기서, Server Options / Serve Modules without Publishing 켜줍니다.
이거 안 하면 파일 수정할 때마다 임시로 자기가 만든 디렉토리로 계속 파일을 복사합니다.
(윈도우 파일 락킹 문제로 이러지 않았을지... =,=)
저는 우분투 쓰기 때문에 이래도 별 문제가 없었는데, 혹시 파일 저장 안 되고 이러시면 다시 꺼주세요.
계속, Publishing / Never publish automatically 켜줍니다.
서빙 디렉토리와 편집 디렉토리가 같으므로 퍼블리쉬할 일이 없으니까요.
Ctrl-S 로 저장하고 닫습니다.

### Run

돌려봅시다. =,=.
Servers 판낼에서 방금 만든 톰켓 설정 지정하고 바로 위에 녹색 플래이 버튼 눌러줍니다.

참고로 이건 팁인데
디버그 버튼 누르시면 코드를 수정하고 서버 Restart 하지 않고 고쳐진 내용을 적용받으실 수도 있습니다.
단, 메서드 인자가 바뀌거나 필드가 추가되거나 이런 변화는 적용할 수가 없습니다. JVM 특성상.
이런 건 어쩔 수 없이 또 서버 리스타트를. =,=

브라우저에 아래 내용을 입력해서 톰켓에 리퀘스트를 보내봅니다.

   http://localhost:8080/hello?name=drypot


결과.

    Hello drypot

### 길다

좀 길었는데요, 우울하게도 너무 길고 복잡하지만 다행스러운 것은 이 프로세스를 프로젝트 초기 세팅시만 실시하면 된다는 겁니다.
다음부터는 Maven 이나 Eclipse, Tomcat 설정 관련 부분 빼고 스프링에 대해서만 부분 부분 코멘트하겠습니다.
매우 짧아지겠네요.


{:class="go-to-index"}
[Java Spring](index)
