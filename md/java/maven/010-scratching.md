# Maven

2009.10.28

Grails 빌드 시스템이 간단하긴 한데 먼가 좀 찜찜해서, =,=
Grails 빌드 시스템 버리고 이클립스 빌드 시스템 쓸 수 있나 돌아다니다가
욕 얻어 먹는 제 3 의 빌드 시스템이 또 있길레 (Maven) 어제부터 보는 중. =,=

Maven is Evil 로 구글했더니 먼가 많이 나오긴 하는데,
구리구리하지만 이게 현재 최고다 이런 분위기네.

결말이 보이지 않는 애니 처럼,
내가 만들 코드는 몇 뼘 되지도 않는데
계속 미궁 속으로 빠져들고 있음. =,=

이클립스 IAM 설치하다가 시간 걸리길레 잠시 낙서. =,=

2009.11.19

좋은 거는 라이브러리를 암 생각없이 땡겨 준다는거.
나쁜거는 나머지 전부인듯. ^^

당연하겠지만 IDE 내장 Make 보다 열라 느려서,
IDE 내장 Make 쓸라니 Maven 프로젝트 모델하고 IDEA 프로젝트 모델하고 두벌 관리해야. =,=

글고 maven 기본 디렉토리 구조 허벌나게 장황해서 이거 다 줄여서 설정하려니
플러그인 마다 문서 보러 다녀야하네.

일이 주는 건지 느는 건지. ^^

일단 pom.xml 한벌 만들어 놓으면 계속 쓸 수는 있을 것 같은데,

모듈 두개만 되도,
둘다 메이븐 모듈 만들어 쓰는게 좋을까,
메인만 메이븐 모듈하고 나머지는 걍 IDEA 모듈로 관리해야하나 흠, =,=

일단 내일 maven 문서 좀 읽고
쓸때 없는 스테이지 다 날려서 속도 얼마나 올릴 수 있나 봐야겠음, =,=

2009.11.19

쉽게 쉽게 가는 것이 맞는데,
메이븐 들춰봤다가 어떻게 잘 쓸 수 없을까 괜히 헤딩하느라 사서 고생중. =,=

Grails 만든 사람이 maven 은 build 계의 EJB2 다라고 욕하던데 모 맞는 것 같기도 하고, =,=
오늘 까정은 한번 계속 뒤벼보다가 정 답 안 나오면 쉬운 방법을 써야할 듯.

maven lifecycle 커스터마이징 하는 방법을 찾았는데, 좀 무지 귀찮겠음, =,=

http://www.sonatype.com/people/2009/08/create-a-customized-build-process-in-maven/

더불어 maven 동물책을 공짜로 주고 있다, 오 좋은 사람들 같음.

http://sonatype.com/products/maven/documentation/book-defguide

2009.11.19

MVC 나 기타 등등 프레임웍들 처럼 메이븐도 라이브사이클 틀에 구조적인 문제점이.

http://portals.apache.org/jetspeed-2/maven/the-need-for-a-custom-lifecycle.html

X 윈도우 시스템 시절부터
프레임웍이 항상 기능을 제공해야지, 정책을 제공하면 망한다는 명언이 있었는데
거 왠만하면 맞는 것 같음.

라이브러리만 세팅하게 하고 걍 IDEA 메이크 써야할 듯. =,=

2009.12.23

http://netbeans.org/kb/docs/java/maven-dependencies-screencast.html

수동 라이브러리 세팅하다가 이런 동영상 보면 Maven 좋아보이는데,
또 쓰려면 그 엄청나게 길게 떨어어지는 XML 코드에 급 좌절.

ant 에서는 copy 같은거 한 줄로 되더만 maven 에서 copy 한번 하려면 후덜덜,

IDE 들 이상하게 Ivy 기본 제공을 안 해준다.

2010.01.23

maven 시작한지 3 개월만에 드뎌 좀 익숙해졌음.
프로젝트 디펜던시부텀 빌드까지 maven 으로 모두 처리하고 있음.

통합환경 계속 이사다녔더니 걍 maven 에 기대고 있는게 편해졌음.
maven 설정하면 나머진 통합환경에서 알아서 지가 맞춤.

지금 multi module 구성인데 여기서 하나 문제가
war 생성하는 것을 못하게 할 수가 없음.

하나 짜리 웹 모듈이면 mvn compile war:exploded 이거 넣을 텐데 멀티 모듈이라 넣을 수가 없고,
mvn package 하면 웹 모듈에서는 무조건 war:war 실행되고 저거 디폴트를 바꿀 수가 없게되어 있음. =,=

할 수 없이 war 처음에 하나 떨구고, 그거 read-only 로 마킹해버렸음. =,=

빌드할때 막판에 하나 에러 나지만 뭐... 무시... ㅋ

maven pom.xml 정의하면 Intellij 에서 자기 빌드 시스템을 maven 과 완전 호환되게 맞추는데,
Intellij 빌드 시스템 쓸 수 없는 것이 groovyc 에 좀 짜증나는 버그가 새로 생긴 것 같음.
이거 나만 그러나. 왜 안 고쳐주지. =,=

maven 으로 돌리면 java stub 만들면서 저 버그 사라진는 것 같음.

이 문제 스칼라로 이사가서 없어지면 좀 편하게 작업할 수 있을 듯.

2010.01.24

maven 때문에 얻은 생각지도 않은 장점은
통합환경 이사 다님에 상관 없이 프로젝트 구조가 그대로 유지된다는 거. =,=
역시 모두 지원하는 포멧은 이런 면이 좋음.

eclipse, netbeans, idea 각자 빌드 시스템 쓰면
출력 디렉토리, 소스 구조 이런거 다 달라서 수작업으로 맞춰야 해서 이사 다닐 때 신경쓰였는데,
maven 쓰닌까 src, target 아래는 모든 환경에서 동일하고,
루트 디렉토리에만 환경저장 파일들 몇 개씩 생김.

디펜던시 정의도 pom.xml 에다 해주면 뭐 알아서들 가져다 적용하고,
빌드는 각 환경에 따라 mvn compile war:inplace 부르는 방법이 다른데 찾아서 이거 지정해주면 끝.

이클립스는 안 쓴지 오래되서 모르겠고,

IDEA 는 maven 관련 기능은 많이 지원하는데 치명적으로 goal 저렇게 두개 넘기는게 핫키 지정이 안 됨. =,=
메이븐 툴 윈도우에서 compile, war:inplace 선택하고 실행 화살표 눌러야. =,=

NetBeans 에서는 maven 툴 윈도우 같은거 지원 안 하고,
메이븐 명령 실행하려면 프로젝트 선택하고 -> Cutome -> Goal 불러서 거기다 또 손으로 적어줘야 하는데,
이게 이걸 쓸 필요가 없는 것이, 기본 Build 나 Run 명령에 maven 수식을 대입해 둘 수가 있음. =o=
build 에 compile war:inplace 지정해 두면 한큐에 바료 호출. 아주 편함.

소나타입에서 공짜로 뿌리는 책들.

http://www.sonatype.com/documentation/books

2010.01.29

소나타입에서 나온 Maven 문서들을 일독하였는데,
빌드사이클이나 기본 phase 에 연결되는 goal 을 수정하려면 플러그인을 만들어야 하더군요. =,=
이방법은 최후의 수단으로 써야겠어서 미루어 두고,

POM parent/child 와 module 개념이 서로 직접적인 연관이 없다는 것을 알았습니다.
그래서 혹시나 하여 아래와 같은 실험을 하였는데 성공하였습니다.

Ant 로 멀티 모듈 프로젝트 만들 때는 프로젝트를 두 개만 생성하면 됩니다.

근데 Maven 튜토리얼에 보면 어그리게이터까지 해서 프로젝트 3 개를 쓰는 예제들만 나옵니다.
이걸 두개로 줄여보았습니다.

어그리게이터를 없애고,
Web Project 의 package 를 war 에서 pom 으로 변경합니다.
Web Project 에 Logic Project 를 module 로 연결합니다.
Web Project 의 package 가 pom 으로 바뀌었으므로 기본 빌드사이클을 돌지 않는다는 장점이 생겨버렸습니다 !
Web Project 의 packge phase 에 war:inplace goal 이나 war:exploded goal 을 연결합니다. war 파일 안 생깁니다 !

Logic Project 는 혼자도 패키징 될 수 있도록 정리해둡니다.

이 상태에서 Web Project 에서 package 하면 결과가 완벽하게 떨어집니다.

여기 까지는 module 관계만 설정했을 뿐 pom parent/child 관계는 설정하지 않았습니다.
혹시 디펜던시나 버전 때문에 parent 를 설정해야할 경우 별도의 pom 파일을 만들어서 연결해야합니다.
Logic Project 에서 Web Project 로 parent 관계를 설정해 버리면 레퍼런스 사이클이 생겨버려서 에러가 납니다.

packaging 은 되는데 Web Project 의 package 타입이 pom 이므로
NetBeans 의 Run/AutoRedeploy 기능이 동작을 안 합니다.
근데 다행스럽게도 NetBeans 에 프로젝트의 타입 힌트를 줄수가 있습니다.
아래 프로퍼티를 profiles.xml 에 넣어줍니다.

	<netbeans.hint.packaging>war</netbeans.hint.packaging>

모든 문제가 해결되었습니다 !!! ^^
POM 파일은 딱 두개만 만들어서 깔끔해졌고,
Web Project 를 Run 하면 Logic Project 가 알아서 만들어지고 끌려들어옵니다.
쓸때없이 War 파일 만드는 일도 없어졌습니다.
NetBeans 는 Web Project 를 war 타입으로 인식해서 자동 디플로이하고 실행해줍니다.
Scala 경우 소스 고치고 다시 F6 누르면,
Java 경우 소스 고치고 Save 만 하면, 자동으로 디플로이 됩니다.

2010.03.11

Maven 노우하우가 계속 쌓이면 좀씩 계속 쉬워지고 있음.

다른 빌드나 프로젝트 관리 시스템에서는 먼가 새로 프로젝트 만들면 또 첨부터 인클루드하고 삽질해야하는데,
Maven 은 정보를 모두 pom.xml 에 적어야하니 첨엔 참 막막하고 황당하지만,
조금씩 적은 것이 계속 모여서 요즘엔 먼가 새로 일 벌리는 것이 점점 빨라지고 있음.

다른 빌드 시스템으로 편하게 하던거 첨에 방법 몰라서 엄청 헤멧는데 결국 메이븐식 방법들 있는 것 같고,
추가로 다른 빌드시스템에서 잘 하지 못했던거 쉽게 되니 좋은 것 같음.

무엇보다 라이브러리 이름만 적어주면 인터넷 스토리지에서 알아서 끌고와 링크해주는거 너무 좋고,
소스코드, 문서도 알아서 같이 끌고 오고, 이건 진짜 대박 같음.

라이브러리 버전업 되면 버전 숫자 올려 적어주면 또 알아서 오고,
필요한 라이브러리 알아서 모아가지고 내 코드랑 묶어서 jar 하나 딱 떨궈주는거 넘 좋은 것 같음.

Scala 컴파일러 관련해서 몇 줄 적어주면 컴파일러 세트 알아서 끌고와 컴파일하고,
Jetty 도 몇 줄 적어주면 알아서 끌고와 웹서버 기동됨. =o=

자바 jar 관리하는게 너무 빡쳐서, Maven 없으면 진짜 지지.

2010.05.02

이해할 수록 참 잘 만든거 같음.
지금 보면 별거 아니지만, 이거 첨에 만들 생각한 사람들도 대단.
절차적인 방법으로 빌드하던 시대에 이렇게 오브젝트 구성으로 프로젝트 컨셉 바꾸기가 쉽지 않았을 껀데.

매우 OO 적이고 FP 적이라,
Class 구성할 때 하는 고민이 POM 구성할 때 그대로 생기고, 해결 법도 비슷함.
inhertance 로 해결할 건지 composition 으로 해결할 건지 하는 것도 비슷하고,
FP 에서 closure 로 절차적인 부분 빼내서 격리하는 거 plugin 구성에 그대로 가져와 있음.

문제는 maven 기본 컨셉이 탄탄한데
이걸 괜히 어설프게 설명한 책들 + 블로그들 때문에 그간 더 머리만 복잡했음. =,=
걍 소나타입에서 나온 컴플리트 레퍼런스 보면 머리가 깨끗해짐. =,=
레퍼런스라고 붙어있긴 한데 컨셉이 제일 깔끔하게 정리되어 있음.

멀티 모듈 만드는 것도 다른 책들 보면 설명 순서를 이상하게 해서리,
개념도 안 깔아주고 하는 방법만 예제 던져 주고 그런데,
멀티 모듈 설명전에 inheritance, composition 비교 부터 해주니까 좋네.

다른 작업할 때 기존에 삽질했던거 그대로 쓰려고 모듈 분리 중인데,
maven pom 이 계승, 콤포지션이 다 되서...
빌드 스크립트도 대부분 그대로 계승해서 쓸 수 있게 되었음.
기대도 안 했던 건데... 횅재. ㅋ

이런거 보면 진짜 기본이 되는 사람들이 먼가 만들어 준 것들이
노가다 줄이는데 좋음.

걍 좀 과도하게 몰입하고 부지런만한 사람들이 만들어 놓은 프레임웍들 보면
뇌 세포만 모잘라지고 일만 더 생김. =,=

*

짤방은 깔끔하게 간출해진 슬릭 pom.xml
mvn package 하면 출력 깔끔하게 나옴.
재 사용 가능한 부분은 모두 엄마한데. 2 단계로 나나놨음.

	<project
		xmlns="http://maven.apache.org/POM/4.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

		<modelVersion>4.0.0</modelVersion>

		<parent>
			<groupId>fa</groupId>
			<artifactId>scala-web</artifactId>
			<version>1.0-SNAPSHOT</version>
			<relativePath>../../../maven/scala-web</relativePath>
		</parent>

		<artifactId>sleek-web</artifactId>
		<packaging>pom</packaging>
		<version>1.0-SNAPSHOT</version>

		<name>sleek-web</name>

		<properties>
			<maven.test.skip>true</maven.test.skip>
		</properties>

		<dependencies>
			<dependency>
				<groupId>com.h2database</groupId>
				<artifactId>h2</artifactId>
			</dependency>
			<dependency>
				<groupId>org.apache.lucene</groupId>
				<artifactId>lucene-core</artifactId>
			</dependency>
			<dependency>
				<groupId>org.apache.lucene</groupId>
				<artifactId>lucene-analyzers</artifactId>
			</dependency>
		</dependencies>

	</project>

