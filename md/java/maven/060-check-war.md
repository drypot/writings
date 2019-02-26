# Maven : War 인지 꼭 확인

2010-11-29

eclipse + m2eclip + tomcat 뭉쳐쓰다보면 이상한 현상들이 많이 나타나서,
내 문제인지 저놈들 문제인지 아니면 다른데서 jar 가 쫑나는 건지 알기가 힘듭니다.

근데 저렇게 묶어 쓸때 maven 프로젝트가 tomcat 에 안 붙는 경우 중 하나가,
pom.xml 에서 package 타입을 jar 로 해놓는 경우입니다.

붙이기 과정 다 하고 플러그인 제대로 설치되었는데도 먼가 잘 안되면
pom.xml 의 package 가 war 로 되어있는지 다시 확인하세요.
