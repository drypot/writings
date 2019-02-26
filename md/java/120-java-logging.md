# Java Logging

2010-11-29 18:31

자바 환경에서 로깅 시스템이 좀 복잡합니다.
뭐가 많아서 선택하기 어렵고 선택해도 간단한 로깅하기 위해 읽으라는 메뉴얼이 수백페이지. =,=

### Log4j

자바에는 전 처리기가 없어서 로그 코드를 심어서 컴파일하면 런타임에 출력 제어를 할 수가 없었습니다.
그래서 1996 년에 런타임에 로그 출력을 제어하기 위해 Log4j 라는 것이 등장하게 됩니다.

### JUL

Log4J 가 잘 되고 있는데 썬에서 JDK 에 JUL (Java Util Logging, Java Logging API) 라는 스텐다드를 만들어 넣습니다.
이게 잘 됐으면 다행인데 후져서 망했습니다. =,=
Log4j 비슷하게 만들었는데 기능이 너무 없어서,,,

문제가 시작됩니다.
어떤 라이브러리는 Log4J 를 사용하고 어떤 라이브러리는 JUL 을 사용하니
라이브러리들이 모이면 로그 출력이 중구난방이 되는 겁니다. =,=
환경 설정도 두가지 해줘야 겠네요.

### JCL

문제를 해결하기 위해 제 3 자가 등장합니다.
JCL (Jakarta Commons Logging, Apache Commons Logging) 은 로깅 API 는 제공하지만 실제 로깅 작업은 Log4J 나 JUL 에 이관합니다.
그러므로 모든 모듈과 라이브러리는 JCL 을 사용하고
실제 로그 엔진이 무엇이 될 지는 한 곳 (보통 웹 서버) 에서만 결정하면 문제가 해결될 것이란 줄거리입니다.
JCL 을 어떤 로그 엔진에 연결할 것인지는 적절한 jar 파일 치환으로 구현되어 있습니다.

### Tomcat JULI

톰켓에서도 이 비슷한 방법을 씁니다.
근데 톰켓은 일을 좀더 복잡하게 만들어서 JCL 을 그대로 쓰지 않고
JCL 가져다가 JUL 에 하드 링크되도록 수정한 JULI 라는 자체 패키지를 사용합니다.
그러니 어쨌든 톰켓 모듈들은 JCL API 을 쓰고 로그 엔진은 JUL 로 설정되어 있습니다.
웹 어플리케이션 입장에서는 JCL 을 쓰면 톰켓 로그 엔진이 Log4J 로 변경되더라도 코드 수정이나 컴파일할 필요는 없습니다.

톰켓 기본 로그 엔진이 JUL 로 되어 있다고
JUL 을 어플리케이션에서 바로 쓰는 것은 좋지 않아 보입니다.
JUL 이 후져서 이것을 다른 엔진으로 바꿀 날이 언젠간 올 수 있기 때문에.

### Tomcat + Log4j

실제로 이것 저거 실험하다가 저는 개발환경 톰켓의 로그 엔진을 할 수 없이 Log4J 로 바꾸었습니다.
보시면 아시겠지만 이클립스에서 톰켓 로그 출력이 두 줄로 나옵니다.
정신이 하나도 없지요. =,=

이것을 축약 출력되게 만들려고 3 일간 삽질했는데요, 결론은 불가능이더군요. =,=
JUL 에서는 출력 포멧을 바꾸려면 스트링 패턴 수정같은 것으로 불가능하고 포멧 클래스를 만들어야 합니다.

이것 까지는 좋은데, 톰켓 클래스 로더 운영이 좀 이상해서, 로그 포멧터 등록이 까다롭고,
쉘 상태에서 되도록 까지는 되었는데, 이클립스 톰켓 플러그인에까지 연결은 안 되는 것 같네요. =,=

결국 그냥 톰켓 로그 엔진을 Log4J 로 바꾸고 문제가 쉽게 해결되었습니다.

### SLF4J & Logback

그런데 여기서 역사가 끝나지 않았고요,
Log4J 만들던 아저씨가 주위에 떨거지가 많아 생겨서 짜증이 나셨는지,
Facade API 레이어와 Real Logging Engine 레이어에 각각 해당하는
SLF4J 와 Logback 패키지 두 개를 새로 만듭니다. =,=

JCL 1.3, 2.0 버전 개발은 사실상 정지되었고요,
현재는 1.2 대 구버전 스터블만 사용 가능한 상태입니다.
그래서 요즘 신규 프로젝트들은 Facade API 로 SLF4J 를 쓰는 분위기 입니다.
유연성도 제일 뛰어난 거 같아서 저도 현재는 이것을 쓰고 있습니다.

로그 엔진도 Logback 이란 것을 만드셨는데
로그 엔진은 어플리케이션 개발자가 선택하는 것이 아니고
톰켓 같은 컨테이너에 선택지가 있기 때문에 아직 Log4J 가 많이 대체된 것 같지는 않습니다.

### 정리

해서, 현재 로깅 관련 패키지들은 아래 처럼 됩니다.

Facade API : SLF4J, JCL  
Real Logging Engine : Logback, Log4J, JUL

### 결론

어플리케이션 개발할 때는 Logging API 로 SLF4J 를 쓰면 되겠습니다.
사용하는 모듈중에 JCL 을 사용하는 것이 있으면 JCL 요청을 SLF4J 로 리다이렉트하는 모듈을 링크해주면 됩니다.

### Maven Dependency

SLF4J API 관련 디펜던시

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-api</artifactId>
		<version>1.6.2</version>
	</dependency>

SLF4J 를 JUL 에 연결하는 모듈

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-jdk14</artifactId>
		<version>1.6.2</version>
	</dependency>

SLF4J 를 Log4J 1.2.x 에 연결하는 모듈

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-log4j12</artifactId>
		<version>1.6.2</version>
	</dependency>

SLF4J 를 stdout 에 연결하는 모듈

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>slf4j-simple</artifactId>
		<version>1.6.2</version>
	</dependency>

JCL 을 SLF4J 로 리다이렉트 해야하면 아래 모듈 추가

	<dependency>
		<groupId>org.slf4j</groupId>
		<artifactId>jcl-over-slf4j</artifactId>
		<version>1.6.2</version>
	</dependency>
