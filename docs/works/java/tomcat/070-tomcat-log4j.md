# Tomcat Log4j

2010-11-29 18:02

Screen shot 2010-10-15 at 14.40.55.pngShow
톰켓 로그 엔진을 Log4J 로 바꾸는 법입니다.
Log4J 로 바꾸게 되면 첨부 파일 처럼 개발시 로그를 보기 좋게 출력할 수도 있고,
서버에서 리모트 머신의 Unix Syslog 데몬에 로그를 쏠 수도 있습니다.

그런데 바꾸는 과정이 좀 지저분하므로, =,=
현재 상태에 만족하시면 그냥 패스하셔도 될 것 같네요.

먼저 tomcat-juli.jar 를 교체해야 합니다.
톰켓 버전별 다운로드 페이지에 가시면 두번째 단락 쯤에 Browse 라는 링크가 보입니다.
잘 찾으셔야합니다. =,=

Browse 하시면 디렉토리 목록 비스무리한 것이 뜨는데,
bin/extra 로 이동하셔서,
tomcat-juli.jar,  tomcat-juli-adapters.jar 를 받습니다.

tomcat-juli.jar 는 CATALINA_HOME/bin 에 해당하는 디렉토리에 존재하는 동명 파일에 덮어쓰시고,

tomcat-juli-adapters.jar 는 CATALINA_HOME/lib 에 해당하는 디렉토리에 넣으셔도 되고,
CATALINA_BASE/lib 에 해당하는 디렉토리에 넣으셔도 됩니다. 취향에 따라.

	log4j.rootLogger=INFO, R

	#log4j.appender.R=org.apache.log4j.RollingFileAppender
	#log4j.appender.R.File=${catalina.base}/logs/tomcat.log
	#log4j.appender.R.MaxFileSize=10MB
	#log4j.appender.R.MaxBackupIndex=10
	#log4j.appender.R.layout=org.apache.log4j.PatternLayout
	#log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n

	log4j.appender.R=org.apache.log4j.ConsoleAppender
	log4j.appender.R.Target=System.out
	log4j.appender.R.layout=org.apache.log4j.PatternLayout
	log4j.appender.R.layout.ConversionPattern=[%t] %m - %c{1}%n

	# %-5p

위에 파일은 Log4J 설정파일입니다.
제가 이것 저것 실험하느라 코멘트 아웃이 좀 있네요.
이것은 CATALINA_HOME/lib 이나 CATALINA_BASE/lib 에 넣습니다

http://logging.apache.org/log4j/1.2/download.html

위에서 Log4J 를 받아서 CATALINA_HOME/lib 이나 CATALINA_BASE/lib 에 넣습니다.

CATALINA_HOME/conf/logging.properties 나
CATALINA_BASE/conf/logging.properties 는 확장자를 바꾸시던지 삭제하시던지 합니다.

이 파일들은 JDK Logging 설정파일입니다.

톰켓 재시작.

*

흠, 근데 이클립스에서 톰켓 인스턴스를 이상하게 관리해서
기본대로 하면 CATALINA_BASE 위치가 흉칙해 지니,

위에 설정들은 CATALINA_HOME 에 적용하시거나,

이클립스 톰켓 인스턴스 디렉토리를 수작업 지정하시면 되는데,
이클립스에서 CATALINA_BASE 를 깨끗하게 수작업 지정하시려면,
톰켓 서버 생성하시고, 프로젝트를 서버에 연결하시기 전에,
톰켓 서버 선택후 F3 으로 설정 오픈하시고 Server Locations 를 3 번째 Use Custom location 으로 바까주시면 됩니다.
