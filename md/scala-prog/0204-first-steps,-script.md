---
title: First Steps, Script
---

2011-07-08 23:22

스칼라는 큰 스케일의 시스템을 작성하기 위해 디자인했지만 스크립팅을 하는데도 유용하게 사용할 수 있다.
다음을 hello.scala 라는 파일에 저장하고,

    println("Hello, world, from a script!")

실행해 보자.

    $ scala hello.scala
    Hello, world, from a script!

(유닉스라면 #! 을 사용해 scala 코멘드 없이 실행할 수 있다.)

코멘드 라인 인자는 어레이 args 를 통해 사용할 수 있다.
어레이 인덱스는 0 부터 시작한다.
어레이 구성요소는 ( ) 를 통해 접근한다.
그러므로 steps 라는 어레이의 첫 요소는 steps(0) 이다. 자바에서 처럼 steps[0] 이 아니다.

( [ ] 대신 ( ) 를 사용하는 데는 깊은 뜻이 있다. 인상쓰지 말자.)

다음을 새로운 파일 helloarg.scala 에 저장하고,

    // Say hello to the first argument
    println("Hello, "+ args(0) +"!")

실행해 보자.

    $ scala helloarg.scala planet
    Hello, planet!

여기서는 "planet" 을 쉘에서 인자로 전달했고 args(0) 을 통해 사용했다.

스크립트에 주석을 포함시킨 것을 눈여겨 보라.
스칼라 컴파일러는 // 와 EOL, 그리고 /* 와 */ 사이에 있는 문자들을 무시한다.

스트링을 + 연산자로 합하는 것도 보라.
"Hello, " + "world!" 는 "Hello, world!"가 될 것이다.


{:class="go-to-index"}
[Programming in Scala](index)
