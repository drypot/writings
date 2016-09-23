# Tomcat One Binary Multiple Instance

2010-11-30 10:53

어플리케이션 패키지 바이너리 하나로 여러 인스턴스의 웹 어플리케이션을 띄워야할 경우가 있습니다.
무식하게 만든 게시판 패키지로 여러 웹 사이트를 운영하는 경우등. =,=

앞단에 nginx, apache 등을 띄워놓은 경우 톰켓 서버 인스턴스는 포트로 구분하면 됩니다.

웹 어플리케이션별로 설정인자를 전달해야할 터인데 이것은 톰켓 설정 파일중 <Context><Parameter> 태그를 사용해 적고
ServletContext 오브젝트를 경유해 전달 받으면 됩니다.

	<?xml version='1.0' encoding='utf-8'?>

	<Server port="8002" shutdown="SHUTDOWN">

		<Service name="drypot-sleek">
			<Connector port="8202" protocol="org.apache.coyote.http11.Http11NioProtocol" maxPostSize="536870912" URIEncoding="UTF-8" enableLookups="false"/>
			<Engine name="drypot-sleek" defaultHost="localhost">
				<Host name="localhost" autoDeploy="false">
					<Context path="/" docBase="/home/drypot/web-site/drypot-sleek/web-site" cachingAllowed="false">
						<Parameter name="appConfigFile" value="/home/drypot/web-site/drypot-sleek/config/sleek-a2.xml"/>
					</Context>
				</Host>
			</Engine>
		</Service>

		<Service name="drypot-drypot">
			<Connector port="8204" protocol="org.apache.coyote.http11.Http11NioProtocol" maxPostSize="536870912" URIEncoding="UTF-8" enableLookups="false"/>
			<Engine name="drypot-drypot" defaultHost="localhost">
				<Host name="localhost" autoDeploy="false">
					<Context path="/" docBase="/home/drypot/web-site/drypot-sleek/web-site" cachingAllowed="false">
						<Parameter name="appConfigFile" value="/home/drypot/web-site/drypot-sleek/config/sleek-drypot-a2.xml"/>
					</Context>
				</Host>
			</Engine>
		</Service>

	</Server>

>

	@Component
	public class App implements ApplicationContextAware {
		private ServletContext servletContext;

		@PostConstruct
		public void init() {
			String appConfigFilePath = System.getenv("APP_CONFIG_FILE");
			if (appConfigFilePath == null) appConfigFilePath = servletContext.getInitParameter("appConfigFile");
		}

		@Override
		public void setApplicationContext(ApplicationContext applicationContext) {
			servletContext = ((WebApplicationContext)applicationContext).getServletContext();
		}
	}
