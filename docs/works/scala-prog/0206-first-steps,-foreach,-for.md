---
title: First Steps, foreach, for
---

2011-07-08 23:44

인식하지 못했을 수도 있겠지만 이전 단계에서는 while 루프를 사용해서 절차적 방식(imperative style)으로 프로그래밍 해보았다.
절차적 방법은 자바, C++, C 등에서 사용하는데  한번에 한가지 명령을 내리고, 루프를 사용해 반복을 하고,
때때로 (전역 변수나 IO 등) 펑션 간에 공유하는 상태를 사용하기도 한다.

스칼라로 절차적 프로그래밍을 할 수도 있지만
스칼라를 알아갈 수록 절차적 방식보다 F 적 방식으로 프로그래밍하는 스스로를 발견하게 될 것이다.
사실 이 책의 주요 목적중 하나는 독자가 F 적 방식에 적응할 수 있도록 돕는 것이다.

F 언어의 주요 특징중 하나는 펑션이 핵심 구문이라는 것이다. 스칼라에서도 그렇다.
명령행 인자를 출력하는 아주 간단한 다른 방법은 아래와 같다.

    args.foreach(arg => println(arg))

이 코드는 args 의 foreach 메서드를 부르고 메서드 인자로 펑션 arg => println(arg) 을 넘기고 있다.
넘기는 펑션은 펑션 리터럴로 그 자리에서 만들었다.
이 펑션은 arg 라는 인자를 가지고 있고 몸체는 println(arg) 다.
이 코드를 pa.scala 라는 파일에 저장하고 실행하자.

    $ scala pa.scala Concise is nice
    Concise
    is
    nice

위 예에서 스칼라는 arg 인자의 타입을 String 으로 추론하였다.
foreach 를 적용하고 있는 배열 args 의 원소 타입이 String 이기 때문이다.
원한다면 인자 타입을 명시적으로 적어줄 수 있는데, 이렇게 하려면 인자 부분을 ( ) 로 감싸야 한다.

    args.foreach((arg: String) => println(arg))

명시적인 것 보다 간략한 것을 좋아한다면 스칼라의 특별 단축 구문을 이용할 수 있다.
펑션 리터럴이 한 문장으로 구성되고 인자를 하나만 받는다면, 인자를 모두 생략할 수 있다.

    args.foreach(println)

이 단축 구문은 partially applied function 이라고 하는데 8.6 절에서 설명한다.

요약하면 펑션 리터럴은 괄호 안에 인자 이름, 오른쪽 화살표, 펑션 몸체로 구성된다.

[79p Figure 2.2 스칼라 펑션 리터럴 문법]


내 친구 for 는 어디 갔을까?
스칼라에서는 보다 F 적인 방향으로 사용자를 인도하기 위해 절차적 for 의 F 적인 친척만이 존재한다.
이를 for 수식이라 부른다.
7.3 절에서 for 수식의 최대 파워를 보게 되겠지만, 전에 여기서 간단히 맛은 보도록 하자.

    for (arg <- args) println(arg)

for (arg <- args) 에서 <- 는 in 으로 읽는다. 그러므로 전체는 for arg in args 로 읽는다.
화살표 왼쪽의 arg 는 val 이다. var 가 아니다.
이 자리의 변수는 항상 val 이기 때문에 val 을 명시적으로 적진 않는다.
val 이기 때문에 for 블럭에서 이 값을 변경할 수도 없다.


### 참고

스칼라에는 자바 계열의 절차적 for 구문이 없다.
스칼라의 for 는 파이선의 list comprehension 에 가깝다.
정 쌩 루프를 돌리려면 while 을 써야한다.
충격적으로 들리겠지만 break 구문도 2010 년에야 추가 되었다.


{:class="go-to-index"}
[Programming in Scala](index)
