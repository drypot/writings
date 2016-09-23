# Multi-Module Web Project

2010-11-29

사이트 작업이 커지면 로직을 위한 자바 코드는 별도 프로젝트(모듈)로 분리하게 됩니다.
이클립스에서는 이 단위를 프로젝트가 칭하고 Maven 이나 IDEA 에서는 모듈이라 칭하는데,
문맥에 따라서 이해해주세요.

멀티 모듈 프로젝트를 빌드하기 위해 메이븐 정석을 따르면,
로직 모듈을 Jar 로 패키징하고(mvn package), 로컬 머신 메이븐 리포지터리에 등록한 이후에만 (mvn install),
이를 사용하는 웹 프로젝트에서 불러쓸 수 있습니다.
개발 과정에서 이 사이클을 계속 돌려야하면 짜증이 나겠지요.

다행이 이클립스 메이븐 플러그인이 똑똑해서
같은 워크스페이스에 열려 있는 메이븐 프로젝트에 대해서는
패키징과 로컬 리포지터리 인스톨 과정을 패스하고,
프로젝트끼리 아주 적절하게 연결을 해줍니다.

구체적으로는 각 프로젝트의 pom.xml 을 분석해서
참고하고 있는 다른 메이븐 프로젝트가 워크스페이스 상에 별견됐을 시
그것을 이클립스 프로젝트 정보로 매핑해주니까
일반적인 이클립스 개발 사이클을 타면 됩니다.

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

		<modelVersion>4.0.0</modelVersion>

		<groupId>net.directmedia</groupId>
		<artifactId>mvc-demo-logic</artifactId>
		<version>1.0-SNAPSHOT</version>
		<packaging>jar</packaging>

		<dependencies>
		</dependencies>

	</project>

로직을 담을 jar 모듈을 정의하는 pom.xml 예입니다.

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

		<modelVersion>4.0.0</modelVersion>

		<groupId>net.directmedia</groupId>
		<artifactId>mvc-demo</artifactId>
		<version>1.0-SNAPSHOT</version>
		<packaging>war</packaging>

		<dependencies>
			<dependency>
				<groupId>net.directmedia</groupId>
				<artifactId>mvc-demo-logic</artifactId>
				<version>1.0-SNAPSHOT</version>
			</dependency>
		</dependencies>

	</project>

위 로직 모듈을 불러쓰는 웹 사이트 프로젝트 예제.
메이븐 상식대로 그룹, 아티펙트, 버전을 적어주면,
메이븐 플러그인이 이클립스 프로젝트 정보로 쫙 매핑합니다.

더 군다나 좋은 것은 이 이클립스 프로젝트 정보가 톰켓 플러그인까지 흘러가니까,
별도 클래스패스 지정하지 않고도 로직 모듈 관련 클래스들이 이클립스 톰켓에 다 링크됩니다.

메이븐으로 패키징을 한다면
로직 모듈에 대해 install 을 해서 로컬 리포지터리에 jar 를 설치한 후
웹 프로젝트에 대해 package 를 해야 메이븐이 로직 모듈을 찾습니다.

이런 식으로 메이븐 리포지터리 운영을 회사내로 넓히고,
각 개발자가 자기가 맡은 jar 를 회사 메이븐 리포지터리에 올리는 프로세스가 만들어 지면
메이븐식 협업이 될 겁니다.

SVN 으로 소스코드 래벨의 협업이 있을 수 있고,
메이븐으로 Jar 레벨 협업이 있을 수 있고.

*

이클립스 파일 메뉴에서 Export 하는 것으로도 War 패키징 될 터인데,
그것은 아직 제대로 써보질 않아서,,,
