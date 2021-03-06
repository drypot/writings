---
title: Classes and Objects, Semicolon Inference
---

2011-07-14 22:09

### Semicolon inference / 세미콘롤 추론

스칼라 프로그램에선 문장 끝에 오는 ; 선택적이다.
원한다면 입력해도 좋지만 한 줄에 한 문장이 온다면 입력할 필요가 없다.
한 줄에 여러 문장을 입력하는 경우에는 입력해야 한다.

    val s = "hello"; println(s)

여러 행에 걸쳐 문장을 입력하고 싶다면 대부분의 경우 그렇게 해도 좋다.
스칼라가 적당한 위치에서 문장들을 분리할 것이다.
아래는 네 줄로 구성된 한 문장이다.

    if (x < 2)
      println("too small")
    else
      println("ok")

하지만 간혹 의도와 다르게 한 문장이 두 문장으로 나눠질 수 있다.

    x
    + y

위 코드는 x 와 +y 두 문장으로 해석된다.
의도한 바가 x + y 였다면 다음과 같이 ( ) 로 감싸야한다.

    (x
    + y)

아니면 + 를 문장 마지막에 적는 방식을 사용할 수도 있다.
+ 와 같은 이항 연산자를 연쇄적으로 사용할 경우 연산자를 문장 마지막에 두는 것이 일반적인 스칼라 스타일이다.

    x +
    y +
    z

문장 분리 규칙의 세부 규정은 생각외로 매우 단순하면서도 잘 작동한다.
다음 조건일 경우 위 아래 두 문장은 이어진다고 본다.

1\. 행이 . 이나 이항 연산자와 같은 문장 끝에 올 수 없는 문자로 끝난다.
2\. 다음 행이 문장을 시작할 수 없는 단어로 시작한다.
3\. ( ), [ ] 가 닫혀지지 않은 상태로 행이 끝났다.

위 경우가 아니라면 문장은 현재 행에서 끝났다고 본다.


{:class="go-to-index"}
[Programming in Scala](index)
