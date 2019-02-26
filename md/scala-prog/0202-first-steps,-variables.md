---
title: First Steps, Variables
---

2011-07-08 22:58

스칼라에는 두 가지 종류의 변수가 존재한다. val 과 var.
val 은 자바에서 final 변수와 비슷하다.
한번 초기화 되면 val 에는 다른 값을 대입할 수가 없다.
var 는 자바의 non-final 변수와 비슷하다.
var 변수에는 자유롭게 값을 대입할 수 있다.

    scala> val msg = "Hello, world!"
    msg: java.lang.String = Hello, world!

이 문장은 msg 변수를 생성해서 "Hello, world!" 문자열을 대입했다.
스칼라의 문자열은 자바의 String 클래스로 구현되기 때문에 msg 의 타입은 java.lang.String 이다.

자바에서 변수를 선언하는데 익숙하다면 한가지 확연한 차이를 발견할 것이다.
java.lang.String 이나 String 같은 것이 val 정의에 보이지 않는다는 것이다.
스칼라는 사용자가 생략한 타입을 추론할 수 있다. (type inference)
여기서는 msg 를 문자열로 초기화했기 때문에 msg 의 타입을 String 으로 추론하였다.

스칼라 인터프리터나 컴파일러가 타입을 추론할 수 있을 때는 불필요하게 타입을 채우지 말고 비워두는 것이 좋다.
하지만 타입을 명시적으로 적고 싶다면 그렇게 해도 되고 때때로 반드시 그래야 하는 경우도 있다.
명시적인 타입 설명은 사용자가 의도한 타입을 쓰도록 하기도 하고 코드를 읽는 사람들에게 유용한 정보를 제공하기도 한다.

자바에서는 타입명을 변수명 앞에 적지만 스칼라에서는 변수명 다음에 적기 때문에 이를 콜론으로 구분해야 한다.

    scala> val msg2: java.lang.String = "Hello again, world!"
    msg2: java.lang.String = Hello again, world!

java.lang 타입들은 스칼라에서 단순 이름으로도 접근할 수 있기 때문에 다음도 가능하다.

    scala> val msg3: String = "Hello yet again, world!"
    msg3: String = Hello yet again, world!

처음으로 돌아가서, msg 는 일단 정의되었고, 아래와 같이 사용할 수 있다.

    scala> println(msg)
    Hello, world!

msg 는 var 가 아니고 val 이기 때문에 msg 에 다른 값을 대입할 수는 없다.

    scala> msg = "Goodbye cruel world!"
    <console>:6: error: reassignment to val
      msg = "Goodbye cruel world!"
        ˆ

재대입을 하려면 var 을 사용해야 한다.

    scala> var greeting = "Hello, world!"
    greeting: java.lang.String = Hello, world!

    scala> greeting = "Leave me alone, world!"
    greeting: java.lang.String = Leave me alone, world!

인터프리터에서 여러 행에 걸치는 내용을 입력하려면 그냥 다음 행에서 입력을 계속하면 된다.
앞서 입력한 행이 완벽하지 않다면 인터프리터는 '|' 를 출력할 것이다.

    scala> val multiLine =
         |   "This is the next line."
    multiLine: java.lang.String = This is the next line.

먼가 잘못 입력해서 인터프리터가 계속 대기할 경우에는 엔터를 두 번 입력하면 된다.

    scala> val oops =
       |
       |
    You typed two blank lines.  Starting a new command.
    scala>

이 책의 나머지 부분에서는 코드를 읽기 쉬게 하기위해 '|' 문자를 생략할 것이다.


{:class="go-to-index"}
[Programming in Scala](index)
