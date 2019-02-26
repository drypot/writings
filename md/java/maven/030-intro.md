# Maven 개요

2010-11-29

프로그래밍 프로젝트를 진행하려면 파일을 많이 관리해야합니다.
그에 따른 프로젝트 디렉토리 구조 규약이 자연스럽게 필요합니다.

또, 소스코드에서 몇 단계를 거쳐 컴파일을 하고, 패키징을하는 반복 과정이 있기 때문에
이 빌딩 과정을 편하게 해주는 방법들도 필요합니다.

과거에는 이런 프로젝트 구조 + 빌드 사이클 기준을 통합환경에서 제시했습니다. (예: 비주얼튜디오, 이클립스)
아니면 개발자가 자기 맘대로의 프로젝트 구조를 만들고 Make, Ant 등의 빌드 사이클 자동화 프로그램을 썼습니다.

통합환경에서 제시하는 방법은 편하긴 한데 유연성이 부족해서 괴상한 요청에 대응하기가 어렵고,

Make, Ant 등은 유연하긴 한데 프로젝트 구조 표준화가 안 되어서 이쪽 프로젝트하다가 저쪽 프로젝트로 가면
완전히 다른 세상을 경험해야하는 문제들이 발생하였습니다.
특히 여러 프로젝트를 동시에 진행하는 오픈소스 개발자들은 이런 프로젝트 구조 문제에 치를 떨었는데,

그래서 아파치에서 일단의 사람들이 프로젝트 구조 표준화와 더불어 빌드 사이클 자동화를 한방에 해결해보자는
시도를 시작했고 그것이 Maven 이고, 요즘은 소나타입이라는 회사를 만들어서 계속 버전업을 하고 있습니다.

Maven 의 장단에 관해서 논하자면, 특히 Ant vs Maven 의 싸움이 붙으면 끝이 없는데요, 제 관점에서 요약하면,
미래는 Maven 이고요, 현재는 가끔 불편하거나, 아예 먼가 하지 못하는 상황이 자주 발생하지만,
그래도 자바 개발을 앞으로 5 년 이상 한다면 Maven 을 이해하고 사용하는 것이 좋을 것이라 봅니다.

프로젝트를 새로 생성할 때 통합환경에 묶인 프로젝트 타입 대신, 메이븐 프로젝트를 만드시는 것이 좋습니다.
메이픈 프로젝트라면 Eclipse 에서 작업하다가 그걸 그대로 IDEA 로 가지고 가도 변경할 것이 없습니다.
모든 자바 통합한경은 메이븐 프로젝트 타입을 지원하니까요.
현재는 메이븐 프로젝트가 모든 자바 통합환경의 공통 규약처럼 되었습니다.

실용적인 부분에서 몇 가지 짚으면,

이클립스도 자기 프로젝트 관리 개념이 있습니다.
메이븐도 자기 프로젝트 관리 개념이 있습니다.
그리고 이 둘은 완전히 다릅니다. 그래서 잘 안 섞입니다.

메이븐 만든 사람들이 이클립스에 메이븐 지원 기능을 계속 추가하고 있어서, (m2eclipse)
이클립스 프로젝트 관리 메뉴들과 메이븐 관련 메뉴들이 섞여 지저분해 진다는 것을 빼면 그럭저럭 쓸만하긴 합니다.
단지, 어디까지가 이클립스 개념이고, 어디서 부터 메이븐인지 쓰시면서 구분해 생각할 필요들이 있습니다.
아니면 먼가 문제가 발생했을 때 대처가 어려워서,,,

보통은 프로젝트 구조 -> 메이븐 규약을 따름 (디렉토리를 메이븐 표준에 맞춘다는 말입니다)
라이브러리 디펜던시 설정 -> 메이븐 규약을 따름 (라이브러리 설정을 pom.xml 에 한다는 말입니다)
컴파일 -> 메이븐을 쓰면 느리기 때문에, 이건 이클립스 기능을 씁니다. 보통 파일 저장하는 순간 자동 컴파일.
패키징 안 한 상태에서 톰켓에 붙여 테스트 -> 메이븐으로 할 수도 있고, 이클립스 플러그인으로도 되는데 이클립스 방식이 편합니다.
패키징 -> 이클립스의 Export 로 해도 되고, 자주 한다면 메이븐 패키징 기능도 좋습니다.
회사 규모의 라이브러리 관리 -> 이걸 만약 한다면 메이븐 기능들을 써야합니다.

네 보시면 아시겠지만 정신분열이 일어나려고 합니다. 첨에는, =,=
그나마 저는 뭐가 좋은지, 아니면 가능이라도 한지 알려주는 사람이 없어서 삽질을 무자게 했는데요,
(더해서 버전에 따라 기능이 계속 바뀌어서 현재는 도움 안되는 블로그 내용 잔뜩)
딱 저위에 상태가 좋더군요.

윈도우 환경에서 오시는 분들은 이런 지경이 좀 이해가 안 되실 것 같습니다.
비주얼 스튜디오는 한 세트로 오니까요.

자바는 거의 시장통이기 때문에, 그래서 먼가 잡동사니들을 연결하는 기능들이 많이 존재합니다.
Maven 이라던지, 스프링이라던지, JPA 라던지, Java EE 규약이나, 온갖 이클립스 플러그인등.
개발에 집중하는데 이런 연결 기술들 자체가 좀 장애물이 많이 됩니다.
어쩔수 없이 감내하면서 가야하지요.