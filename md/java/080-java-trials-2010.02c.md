# 자바 삽질의 기록 2010.02c

2010-11-28 14:32

Play! 좋은 점은 Servlet 까지 모든 Java EE 스택을 내 버려서 무지무기 가볍다는 것 같다.
Jetty 나 Tomcat 같은 라이트 Servlet 컨테이너도 가지고 있지 않고,
Apache Mina 가지고 웹 서버 기능을 자체구현하고 있다.
Python 의 Django 나 Ruby Rails 와 기능이 딱 동급이다.

이게 단점이 될 수도 있는데 Servlet 도 안 되니 기본적으로는 Java EE 웹 스텍 도움을 전혀 받지 못한다.
play war 명령으로 war 패키징해서 Servlet 컨테이너에 실을 수는 있는데 jar libaray 관리도 그렇고 좀 거석하다.

우리나라는 자바 아니면 안 된다는 분위기이지만,
자가 개발 프로세스의 불편함을 못견뎌서 해외에서는 Rails 나 Django 로 이사가는 경우가 많았던 것 같은데,
Play! 가 이런 이주를 줄일 수 있을 것 같다.

Play! 의 특이한 장점중의 또 한가지는 로컬 변수 User user 같은 것을 render(user) 와 같이 템플릿 인자로 넘길 때
"user" : user 와 같은 매핑이 필요 없다는 것이다.
Play! 에서 스택 트레이스를 해서 로컬 변수 이름을 찾아낸다.

Java 로 거의 PHP 개발 사이클 수준의 작업이 가능할 것 같다.

근데 Scala 쓰려고하는데 아직 Scala 지원에 문제가 좀 있어서 보류중이다.
그리고 Maven 빌드 시스템을 지금 무척 좋아하고 있어서 나 스스로는 옛날 방식 쓸 것 같다. =,=

그래도 Play 보다가 Scalate 라는 템플릿 엔진을 찾을 수 있었는데
(Groovy 처음 개발 시작했던 James Strachan 아저씨가 짐 작업중)
Scalate 가 Ruby 환경에서 쓰이는 Haml 템플릿엔진을 자바 플랫폼으로 구현해 준다.

현재는 nginx + Jetty + Scala + 사제 웹 뼈대 + Scalate 조립하려고 작업중이다.
Scalate 가 현재 1.0 개발중이라 오류가 작열해서 좀 잘 안되고 있는데 일단 방향은 그렇다.

얼마 전까지 GlassFish 가 아주 맘에 들어서 의지하고 있었는데,
nginx + Jetty 로 분해해서 가져가는 방법으로 다시 생각중이니다.
Jetty 가 Tomcat 보다 기능은 좀 없는데, 내부 디자인이 깔끔하게 잘 되어 있어서,
환경 설정을 참 납득할 수 있게 적을 수 있다.
(이 생각은 나중에 많이 바뀌었다.)

GlassFish 가 좀 무거운데 Minimal Pack 이 나왔으면 좋겠다.
하지만 오라클이 가져가서 Oracle GlassFish 로 재브랜딩 되었으니 다이어트될 확률은 0%.