# Maven : 쌩뚱 맞은 jar 파일 추가

2010-11-29

메이븐 센트럴 리포지터리에 있는 jar 는 디펜던시 설정하면 잘 끌려옵니다.
그런데 대한민국의 결제 루틴 같은 쌩뚱 맞은 jar 는 리포지터리에 없기 때문에 평범하게 가져올 수가 없습니다.

이럴 때는 쌩뚱 맞은 jar 를 본인 피시의 로컬 리포지터리에 수동 인스톨하고,
평범한 메이븐 jar 가져오듯이 디펜던시 설정을 하시면 됩니다.

터미널 여시고 아래 구문으로 메이븐 실행시키시면 되고.
그룹아이디, 아티펙트아이디, 버전은 적당히.
패키징은 jar 라고 하시면 될 것 같군요.

	mvn install:install-file -Dfile=<path-to-file> -DgroupId=<group-id> -DartifactId=<artifact-id> -Dversion=<version> -Dpackaging=<packaging>

http://maven.apache.org/guides/mini/guide-3rd-party-jars-local.html