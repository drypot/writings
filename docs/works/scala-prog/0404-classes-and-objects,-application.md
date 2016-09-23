# Classes and Objects, Application

2011-07-16 19:48

### A Scala application / 스칼라 어플리케이션

앞에서는 인터프리터 방식으로 스칼라 코드를 실행하는 방법을 보았다. ex) scala
스칼라 스크립트를 *.scala 파일에 저장하고 실행하는 법도 보았다. ex) scala Script.scala

이번 섹션에서는 스칼라 어플리케이션의 틀을 구성하고 컴파일해서 실행하는 법을 본다.

컴파일된 스칼라 프로그램을 실행시키려면 싱글턴의 이름을 제공해야 한다. ex) scala Summer
여기서 Summer 가 싱글턴의 이름이다.

스칼라 어플리케이션의 시작 지점으로 지정된 싱글턴은 main 메서드를 가져야한다.
main 메서드는 Array[String] 타입의 인자를 받는다.
리턴 타입은 Unit 이다.

	// In file Summer.scala
	import ChecksumAccumulator.calculate

	object Summer {
		def main(args: Array[String]) {
			for (arg <- args)
				println(arg +": "+ calculate(arg))
		}
	}

싱글턴 이름은 Summer 다.
Summer 는 main 메서드를 가지고 있고 어플리케이션 진입점으로 사용되는데 적합한 형식을 하고 있다.

첫번 째 문장은 이전 예에서 사용했던 ChecksumAccumulator 싱글턴의 calculate 메서드를 임포트하고 있다.
이 임포트 문장 덕에 싱글턴의 메서드를 짧은 이름으로 사용할 수 있다.

(
스칼라는 java.lang, scala 패키지의 멤버들과 scala.Predef 싱글턴의 멤터들을 모든 소스 파일에 자동으로 임포트한다.
Predef 는 유용한 메서드를 많이 가지고 있다.
println 은 Predef.println 을 의미한다. (Predef.println 은 다시 Console.println 을 호출한다.)
assert 은 Predef.assert 을 의미한다.
)

스칼라는 자바와 다르게 소스 파일 이름을 클래스 이름과 동일하게 만들지 않아도 된다.
자바에서는 SpeedRacer 클래스 정의를 SpeedRacer.java 파일에 담아야 했다.

하지만 (스크립트 파일이 아닌 경우) 자바에서 처럼 파일 이름을 클래스 이름에 따라 정하는 것이 좋다.
원하는 클래스의 소스 파일을 쉽게 찾을 수 있기 때문이다.

컴파일은 scalac 명령을 사용한다.

	$ scalac ChecksumAccumulator.scala Summer.scala

컴파일을 반복적으로 할 경우 fsc 명령을 사용하는 것이 유용하다. 빠르다.

	$ fsc ChecksumAccumulator.scala Summer.scala

fsc 는 fast Scala compiler 의 약자다.
스칼라 컴파일러는 기동 과정에서 jar 파일들을 읽고 여러가지 초기화 작업을 하므로 시작 시간이 꽤 걸린다.
fsc 는 처음 실행될 때 컴파일러 데몬을 띄우는데 두번 째 fsc 명령 부터는 이미 떠있는 컴파일러 데몬을 사용한다.
fsc 는 컴파일러 데몬에 컴파일할 파일 목록만 제공한다.
fsc 데몬을 중지시키는 명령은 fsc -shutdown.

(
1 년 전에는 fsc 로 컴파일 할 경우 압축을 많이 한 구문에서 알 수 없는 에러가 났다.
이상한 에러가 나면 일반 scalac 로 컴파일 해볼 필요가 있다.
)

scalac 나 fsc 명령은 자바 *.class 파일을 생성한다.
*.class 파일은 scala 명령으로 실행한다.
scala 명령에 *.scala 파일을 주면 스크립트로 인식해 컴파일 한 후 실행하지만
싱글턴 오브젝트 이름을 주면 컴파일된 *.class 파일을 실행하게 된다.
싱글턴의 클래스 이름은 싱글턴 이름 뒤에 $ 가 붙는 규칙이 있다.
싱글턴 Summer 의 클래스 이름은 Summer$ 다.

	$ scala Summer of love

결과

	of: -213
	love: -182


### The Application trait / 어플리케이션 트레잇

scala.Application 트레잇이 있는데 제약이 너무 많아 쓸 수가 없다.
명령행 인자를 받아야 하는 경우에도 쓸 수 없고, 멀티 스레드 프로그램에서도 쓸 수 없다.
생략하도록 한다.


### 참고

scalac 로 컴파일한 클래스 파일을 편의상 scala 로 실행하지만
어짜피 자바 클래스 파일이므로 java 명령으로도 실행할 수 있다.
단, 스칼라 패키지에 따라오는 scala-library.jar 를 연결해 주어야 한다.

Tomcat 이나 기타 프레임웍에서 스칼라 코드를 호출할 때는 일반 자바 코드와 크게 다른 점은 없다.
이때도 컴파일된 사용자 클래스 파일에 추가해서 scala-library.jar 만 연결해 주면 된다.


### 참고

2011.07 현재 IDE 의 스칼라 지원 정도는 IntelliJ IDEA > NetBeans > Eclipse 순이다.
IntelliJ IDEA 는 실무에 큰 부족함이 없을 만큼 스칼라 지원을 잘 하고 있다.
NetBeans 는 다중 모듈 프로젝트 간에 키워드 인덱스 업데이트를 못하는 귀찮은 문제가 있다.
사실 Eclipse 스칼라 플러그인은 지난 1 년간 써보지 못했다.
1 년 전에는 거의 쓸 수가 없을 지경이었다. 요즘은 계속 업그레이드 하고 있는 것 같다.
어쨌든 스칼라 대중화에 이클립스 플러그인은 매우 중요하다.
