# Jetty + Spring + JSP + NoXML

2011-09-05 16:38

고민의 시작은 사용법 괴상한 Tomcat 을 버리자는 것 하나.
web.xml 없애고 spring.xml 도 없애서 자바 코드로 모든 설정을 하자는 것 하나.
웹 컨테이너 위에서 war 를 돌리는 것이 아니라 Servlet 컨테이너를 임베딩해서
평범한 자바 어플리케이션 실행시키듯이 main 메서드로 실행하자는 것 하나였습니다.

줄거리는 이러합니다.
Jetty 서버를 임베딩해서 JettyServer POJO 를 만듭니다.
기존에 web.xml 에서 하던 Servlet Config 를 Java 코드로 대신합니다.
servlet-spring.xml 설정들도 Spring Java Annotation 으로 대신합니다.
기존에 익숙하던 Servlet 모델 개념은 그대로 사용합니다.

기존에 Spring MVC 를 사용하시던 분들은 ContextLoaderListener, DispatcherServlet 에 익숙하실 겁니다.
아래 Jetty 설정 메서드들은 web.xml 에서 하던 것과 완전히 같은 기능을 합니다.
스터틱 파일 서비스를 위해서 Jetty 에 딸려오는 DefaultServlet 을 추가해줬습니다.
JspServlet 도 추가했습니다.


	package com.drypot.sleek.draft;
	
	import org.apache.jasper.servlet.JspServlet;
	import org.eclipse.jetty.server.Server;
	import org.eclipse.jetty.server.nio.SelectChannelConnector;
	import org.eclipse.jetty.servlet.DefaultServlet;
	import org.eclipse.jetty.servlet.ServletContextHandler;
	import org.eclipse.jetty.servlet.ServletHolder;
	import org.springframework.web.context.ContextLoaderListener;
	import org.springframework.web.servlet.DispatcherServlet;
	
	public class JettySetter {
	
		public JettySetter() {
		}
	
		public void configure(Server server) {
			setConnector(server);
			setHandler(server);
		}
	
		public void setConnector(Server server) {
			SelectChannelConnector connector = new SelectChannelConnector();
			connector.setPort(8080);
			server.addConnector(connector);
		}
	
		public void setHandler(Server server) {
			ServletContextHandler handler = new ServletContextHandler(ServletContextHandler.SESSIONS) {
				@Override
				protected boolean isProtectedTarget(String target) {
					while (target.startsWith("//"))
						target = URIUtil.compactPath(target);
					return StringUtil.startsWithIgnoreCase(target, "/web-inf") || StringUtil.startsWithIgnoreCase(target, "/meta-inf");
				}
			};
			handler.setContextPath("/");
			handler.setMaxFormContentSize(512 * 1024 * 1024);
			handler.setResourceBase("/Users/drypot/Documents/java/sleek/drypot-sleek-web/src/main/webapp");
			addContextLoaderListener(handler);
			addDispatcherServlet(handler);
			addJspServlet(handler);
			addDefaultServlet(handler);
			server.setHandler(handler);
		}
	
		public void addContextLoaderListener(ServletContextHandler handler) {
			handler.addEventListener(new ContextLoaderListener());
			handler.setInitParameter("contextClass", "org.springframework.web.context.support.AnnotationConfigWebApplicationContext");
			handler.setInitParameter("contextConfigLocation", "com.drypot.sleek.draft");
		}
	
		public void addDispatcherServlet(ServletContextHandler handler) {
			ServletHolder holder = new ServletHolder(new DispatcherServlet());
			holder.setInitParameter("contextConfigLocation", "");
			holder.setInitOrder(3);
			handler.addServlet(holder, "/");
		}
	
		public void addJspServlet(ServletContextHandler handler) {
			ServletHolder holder = new ServletHolder(new JspServlet());
			holder.setInitParameter("modificationTestInterval", "5");
			holder.setInitParameter("logVerbosityLevel", "DEBUG");
			holder.setInitParameter("sendErrorToClient", "true");
			holder.setInitParameter("compilerSourceVM", "1.5");
			holder.setInitParameter("compilerTargetVM", "1.5");
			holder.setInitParameter("fork", "false");
			holder.setInitParameter("trimSpaces", "true");
			holder.setInitOrder(2);
			handler.addServlet(holder, "*.jsp");
		}
	
		public void addDefaultServlet(ServletContextHandler handler) {
			ServletHolder holder = new ServletHolder(new DefaultServlet());
			//holder.setInitParameter("resourceBase", "");
			holder.setInitParameter("dirAllowed", "true");
			holder.setInitParameter("welcomeServlets", "false");
			holder.setInitParameter("gzip", "false");
			handler.addServlet(holder, "*.css");
			handler.addServlet(holder, "*.js");
			handler.addServlet(holder, "*.ico");
		}
	
		public static void main(String[] args) {
			try {
				Server server = new Server();
				new JettySetter().configure(server);
				server.start();
				server.join();
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}
	
	}
	

servlet-spring.xml 을 대신하는 스프링 설정 클래스입니다.
Jetty 설정부에서 contextConfigLocation 으로 Spring Component 스캐닝 범위를 설정하므로
여기에서는 @ComponentScan 어노테이션을 뺏습니다.
평소처럼 Datasource 등을 여기에 줄줄이 매달면 됩니다.

	package com.drypot.sleek.draft;
	
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.annotation.Bean;
	import org.springframework.context.annotation.Configuration;
	import org.springframework.transaction.annotation.EnableTransactionManagement;
	import org.springframework.web.servlet.config.annotation.EnableWebMvc;
	import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
	
	@Configuration
	@EnableTransactionManagement
	@EnableWebMvc
	public class SpringConfig extends WebMvcConfigurerAdapter {
	
		@Autowired
		ApplicationContext context;
	
		@Bean
		public String someCoolObj() {
			return "The Cool One";
		}
	
	}
	

테스트에 사용했던 Spring MVC 컨트롤러입니다.

	package com.drypot.sleek.draft;
	
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
	
	
다른 글에서 자세히 언급했었는데 Java Code-base Configuration 은 Spring 3.1.0.M2 부터 가능합니다.

Spring : Code-based Configuration For Spring MVC  
http://drypot.com/post/215

Spring 관련 디펜던시는

	<repositories>
		<repository>
			<id>org.springframework.maven.milestone</id>
			<name>Spring Maven Milestone Repository</name>
			<url>http://maven.springframework.org/milestone</url>
		</repository>
	</repositories>
	
	<properties>
		<org.springframework.version>3.1.0.M2</org.springframework.version>
	</properties>
	
	...
	
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
	
Jetty 관련 디펜던시는

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-simple</artifactId>
	</dependency>
	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-servlet</artifactId>
		<version>8.0.0.v20110901</version>
	</dependency>
	<dependency>
		<groupId>org.mortbay.jetty</groupId>
		<artifactId>jsp-2.1-glassfish</artifactId>
		<version>2.1.v20100127</version>
	</dependency>
	
JSP 처리하는 jasper 가 참 문제였는데,
예전 처럼 tomcat 에 포함된 jasper 를 그냥 가져다 쓰지 못합니다.
Jetty 주위에 있는 jasper 수정판이 여러 가지인데 현재로서는 위에 포함한 아티펙트가 제일 쓰기 편합니다.

Jetty 를 쓰지 않고 개발자들에게 익숙한 Tomcat 을 써도
Embedded 스타일 코드를 쉽게 만들 수는 있습니다.
그런데 Tomcat 구조가 참으로 이상하여 당장 해놔야 겠다는 생각은 들지 않고 있는데,
인터넷 검색하시거나 org.apache.catalina.startup.Tomcat 클래스 사용하시면
별로 어렵지 않게(약간의 짜증을 수반? =,=) Tomcat 도 임베딩하실 수 있습니다.
