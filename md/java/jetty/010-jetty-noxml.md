# Jetty + File Server + NoXML

2011-08-30 01:13

Tomcat 은 참 기괴하다. 그래서 자주 다른 곳을 쳐다보게 된다.
일 년 반만에 Jetty 를 들춰봤는데 많이 깔끔해졌다.
ResourceHandler 버그도 사라졌고, Sample 과 문서화도 아주 훌륭해졌다.
이어지는 몇 편의 글은 Jetty 를 Tomcat 대용으로 사용하는 방법이다.
거기에 더불어 구식 Servlet + web.xml + war 패턴을 겉어내고
Jetty + Spring 을 평범한 Java Application 방식으로 만들려고 한다.

Jetty 가 좋은 점은 구조가 아주 간단 명료하다는 점이다.
Host, Port, URL 등의 Context 별로 Request 를 처리하는 Handler Chain Collection,
그리고 데코레이터 패턴으로 구성된 각 Handler Chain 의 Handler 들이 전부다.

다른 글에서 좀 긴 버전을 설명하기 전에 이번 글에는 간단한 File Server 코드를 넣었다.

	package com.drypot.sleek;

	import org.eclipse.jetty.server.Handler;
	import org.eclipse.jetty.server.Server;
	import org.eclipse.jetty.server.handler.DefaultHandler;
	import org.eclipse.jetty.server.handler.HandlerList;
	import org.eclipse.jetty.server.handler.ResourceHandler;
	import org.eclipse.jetty.server.nio.SelectChannelConnector;

	public class JettyLauncher {

		public void run() {
			try {
				Server server = new Server();
				server.addConnector(getConnector());
				server.setHandler(getHandlerList());
				server.start();
				server.join();
			} catch (Exception e) {
				throw new RuntimeException(e);
			}
		}

		private SelectChannelConnector getConnector() {
			SelectChannelConnector connector = new SelectChannelConnector();
			connector.setPort(8080);
			return connector;
		}

		private HandlerList getHandlerList() {
			HandlerList handlerList = new HandlerList();
			handlerList.setHandlers(new Handler[]{
					getResourceHandler(),
					new DefaultHandler()
			});
			return handlerList;
		}

		private ResourceHandler getResourceHandler() {
			ResourceHandler resourceHandler = new ResourceHandler();
			resourceHandler.setDirectoriesListed(true);
			resourceHandler.setResourceBase("/Users/drypot/Documents/test pic");
			return resourceHandler;
		}

		public static void main(String[] args) {
			new JettyLauncher().run();
		}

	}

필요한 디펜던시.

	<dependency>
		<groupId>org.eclipse.jetty</groupId>
		<artifactId>jetty-servlet</artifactId>
		<version>8.0.0.RC0</version>
	</dependency>

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-simple</artifactId>
	</dependency>

아래 위키 페이지에도 나와 있는데 Jetty 모듈은 매우 잘게 나뉘어 있다. 필요한 기능을 가진 아티펙트까지만 연결하면 된다. web.xml + Servlet Packaging 을 쓰려면 jetty-webapp 아티펙트가 필요한데 그렇지 않으면 jetty-servlet 면 된다.

<http://www.eclipse.org/jetty/>
<http://wiki.eclipse.org/Jetty>
<http://wiki.eclipse.org/Jetty/Reference/Dependencies>