---
title: First Steps, Scala Interpreter
---

2011-07-08 22:42

스칼라 코드를 써볼 시간이다.
깊이있는 스칼라 학습을 시작하기 전에 큰 그림을 이해하고
기초적인 스칼라 코드를 학습하기 위한 First Step 단계들을 삽입했다.

예제를 실행하려면 표준 스칼라를 설치해야한다.
아래 페이지에 가서 플랫폼별로 안내를 따르라.

<http://www.scala-lang.org/downloads>{:target="_blank"}

Eclipse, IntelliJ, NetBeans 플러그인도 사용할 수 있다.
하지만 이번 장의 절차를 진행하는데는 scala-lang.org 의 Scala 배포판을 사용하고 있다고 가정한다.
(역자는 IntelliJ 를 강추한다. 무료버전으로도 스칼라 학습이 가능하다.)


### 1 단계 스칼라 인터프리터 사용법 배우기

스칼라를 시작하는 가장 빠른 방법은 스칼라 인터프리터를 사용하는 것이다.
단순히 수식을 인터프리터에 타이핑해 넣으면 수식이 계산되서 결과가 출력될 것이다.
스칼라 인터프리터 이름은 걍 'scala' 다.

    $ scala

    Welcome to Scala version 2.8.1.
    Type in expressions to have them evaluated.
    Type :help for more information.
    scala>

다음과 같은 수식을 입력하고 엔터 때리면,

    scala> 1 + 2

다음이 출력된다.

    res0: Int = 3

res0 는 계산된 값을 저장한다.
result 0 이란 의미인데 이번 경우에는 자동생성 되었다.
사용자가 정의한 이름을 표시할 수 있는 경우도 있다.

여기서 Int 는 scala 패키지에 있는 Int 클래스를 의미한다.
스칼라 패키지는 자바 패키지와 비슷하다.
전역 네임스페이스를 나누고 정보 가림 수단을 제공한다.

Int 클래스의 값은 자바의 int 값에 대응한다.
좀더 일반화해서 스칼라는 모든 자바의 원시 타입들에 대응하는 스칼라  클래스들을 가지고 있다.
예를 들어 scala.Boolean -> boolean, scala.Float -> float.
(스칼라 컴파일러는 자바 원시 타입의 성능 잇점을 얻기 위해 스칼라 클래스들을  자바 원시 타입으로 치환한다.)

resX 들은 재사용될 수 있다.
예를 들어 res0 에 3 이 대입되었기 때문에 res0 * 3 은 9 가 될 것이다.

    scala> res0 * 3
    res1: Int = 9

Hello, World! 를 출력하기 위해선 다음과 같이 입력한다.

    scala> println("Hello, world!")
    Hello, world!

println 펑션은 자바의 System.out.println 처럼 전달 받은 문자열을 stdio 에 출력한다.

인터프리터는 :quit 나 :q 를 입력해서 종료시킬 수 있다.


### 참고

인터프리터는 한 행 한 행이 수행될 때마다 메모리에 임시 클래스를 만들어 컴파일한다.
인터프리터에 입력한 문장은 임시 클래스의 메서드 안에 들어가 실행된다.
모든 것이 클래스 안에만, 실행되는 코드는 메서드 안에만 존재할 수 있는 JVM 환경이기 때문에 이런 절차는 필수 적이다.


{:class="go-to-index"}
[Programming in Scala](index)
