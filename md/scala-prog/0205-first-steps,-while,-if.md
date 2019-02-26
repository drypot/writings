---
title: First Steps, while, if
---

2011-07-08 23:27

다음을 printargs.scala 파일에 입력하고 while 문을 사용해보자.

    var i = 0
    while (i < args.length) {
      println(args(i))
      i += 1
    }

이 스크립트는 var i = 0 변수 정의로 시작한다.
타입 추론을 통해 i 의 타입은 초기값 0 의 타입인 Int 가 된다.
while 구문은 i < args.length 가 거짓이 될 때까지 중괄호 블럭을 반복해서 실행한다.
자바의 ++i 나 i++ 은 스칼라에서 사용할 수 없다.
스칼라에서 변수 값을 증가시키려면 i = i + 1 을 쓰거나 i += 1 을 써야한다.
이 스크립트를 다음 명령으로 실행시켜보자.

    $ scala printargs.scala Scala is fun
    Scala
    is
    fun

다음 코드를 echoargs.scala 파일에 입력한다.

    var i = 0
    while (i < args.length) {
      if (i != 0)
        print(" ")
      print(args(i))
      i += 1
    }
    println()

이 버전에서느 println 을 print 로 바꾸어 봤다.
모든 인자들은 같은 행에 출력될 것이다.
인자마다 앞에 공백을 추가하고 있는데 if (i != 0) 구문을 통해 첫 번째 인자는 제외했다 .
결과가 좀더 읽기 편해 졌다.
마지막에 println 을 추가해서 모든 인자를 출력한 후 개행문자를 출력하게 했다.

    $ scala echoargs.scala Scala is even more fun
    Scala is even more fun

스칼라에서는 자바에서 처럼 while 과 if 를 위한 불린 수식을 ( ) 안에 넣어야 한다.
루비처럼 if i < 10 와 같이 쓸 수 없다.
블럭이 한 문장만 가지고 있을 때는 { } 를 생략할 수 있다.
아직 예로 보진 못했지만 스칼라도 자바처럼 문장을 ; 으로 구분할 수 있다.
하지만 대부분의 경우에 필수 요소는 아니다.
좀 정신 산란한 것을 좋아한다면 echoargs.scala 를 다음과 같이 적었을 수도 있다.

    var i = 0;
    while (i < args.length) {
      if (i != 0) {
        print(" ");
      }
      print(args(i));
      i += 1;
    }
    println();


{:class="go-to-index"}
[Programming in Scala](index)
