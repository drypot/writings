# Glassfish

2009.12.18

Java EE 6 / Glassfish v3 정식 나와서 설치함.

Preview 때 문제 없어졌다.
같은 포트에 Virtual Host 여러개 연결할 수 있는 것 확인.
버추얼 호스트 생성하는데 docRoot 에 딱 연결하니까 바로 쌩하고 돔.
전엔 먼가 지저분한 것 마구 생성했었는데 그런 증상 안 보임.

버추얼 호스트 되니 Glassfish 로 급 쏠리고 있는데,
Glassfish 에서 쓸 기능은 Grizzly 가 제공하는 웹 기초 기능 + Servlet 정도까지 이고,
나머지는 모두 잉여지만, 이게 이쪽으로 쏠릴 수 밖에 없는 것이,

Tomcat 이 관리가 너무 이상함. =,=
작동은 하나 서버라고 보기에는 너무 부족함.

젤 많이 쓰고 중요한 부분인데 Tomcat 은 너무 방치된 것 같음. 누가 깡그리 정리를 좀 해줬으면.
얼핏보니 톰켓 첨에 만든 아저씨 Ruby 하러 도망갔다고 하는 것 같던데. =,=

2009.12.25

JBoss, WebLogic, WebSphere 같은 놈들은 못 써봐서 비교 평가는 못하겠고,
걍 GlassFish 만 놓고 봤을때 기본 기능 충분하고 관리 도구들 충실해서 만족중.
Java 웹 개발할 때 괜히 Jetty / Tomcat 쓰면서 고생할 필요 없어진 것 같음.

2009.12.26

https://glassfish.dev.java.net/ 여기서 배포하는 glassfish 에는 API Javadoc 이 없다. =,=
썬에 가서 Java EE 6 SDK 받아야함, 결국, =,=

IDEA + Tomcat 프로젝트를 NetBeans + GlassFish 프로젝트로 옮기고 있는데
중간에 불필요한 스텝을 더 밟는데도 NetBeans 빌드가 훨 빠름.

IDEA 에서는 파일하나 수정하면 Groovy 파일 전부 컴파일 했는데 NetBeans ANT 는 하나씩만 컴파일 함.
Groovy 파일간에 디펜던시 확인은 하는건가,

Servlet 3.0 에 FileUpload 기능 들어갔다고 좋아했는데 API 디자인이. =,=
왜 일반 메서드로 처리해야할 껄 Annotation 으로 하지. =,=
런 타임에 결정될 값을 Annotation 으로 지정하게 되어있음. =,=

어우 진짜,
