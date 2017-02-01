---
title: Types and Operations, Operator Precedence
---

2011-07-17 23:05

### Operator precedence / 연산자 우선순위

연산자 우선순위는 수식의 어떤 부분이 먼저 계산될 것인지를 결정한다.
2 + 2 * 7 에서 * 연산자가 우선순위가 + 보다 높기 때문에 * 부분이 먼저 계산된다.
우선순위를 바꾸기 위해서는 (2 + 2) * 7 과 같이 ( ) 를 사용한다.

스칼라에는 연산자가 없고 메서드를 연산자 표기법으로 사용하는 것 뿐이다.
그렇다면 우선순위는 어떻게 처리할까?

스칼라는 연산자 표기법에 사용된 메서드의 첫 글자에 따라 우선순위를 결정한다.
메서드 이름이 * 로 시작되면 + 로 시작되는 메서드보다 높은 수선순위를 갖는다.
a +++ b *** c 는 a, b, c 가 변수이고 +++, *** 이 메서드일 때 a +++ (b *** c) 가 된다.

Operator precedence

    (all other special characters)
    * / %
    + -
    :
    = !
    < >
    &
    ˆ
    |
    (all letters)
    (all assignment operators)

<< 메서드는 < 로 시작하기 때문에 + 보다 우선순위가 낮다.
그러므로 위 수식은 2 << (2 + 2) 가 된다.

    scala> 2 << 2 + 2
    res41: Int = 32


이 규칙에는 한 가지 예외가 있다.
연산자가 = 으로 끝나면서 비교 연산자 (<=, >=, ==, !=) 가 아닐 때는 단순 대입 연산자 = 과 같은 우선순위를 갖는다.
즉, 어떤 연산자보다도 우선순위가 낮다.
x *= y + 1 는 x *= (y + 1) 이 된다.


### Operator associativity / 연산자 결합방법

스칼라에서 연산자와 피연산자의 결합, 연산자와 연산자의 결합방향은 연산자의 마지막 글자에 의해 결정된다.

끝이 : 으로 끝나는 메서드는 오른쪽 피연산자에 대해서 왼쪽 피연산자를 인자로 호출된다.
그외의 경우는 일반적인 형태로 왼쪽 피연산자에 대해서 오른쪽 피연산자를 인자로 호출된다.

a * b 는 a.*(b) 가 된다.
a ::: b 는 b.:::(a) 가 된다.

어떤 결항방법이 사용되든 피연산자들은 항상 왼쪽부터 오른쪽으로 계산된다.
그러므로 a ::: b 는 엄밀히 말하자면 { val x = a; b.:::(x) } 과 같다.

`:` 룰은 같은 우선순위의 연산자가 줄지어 있을 때 연산자 끼리의 결합 방향에도 영향을 미친다.
메서드 끝이 : 으로 끝나면 연산자들은 오른쪽에서 왼쪽 방향으로 뭉친다.
그 외에는 왼쪽에서 오른쪽으로 뭉친다.

`a:::b:::c 는 a ::: (b ::: c) 가 된다.
a * b * c 는 (a * b) * c 가 된다.

연산자 우선 순위를 이해하고 활용하는 것에 주저할 필요는 없다.
하지만 ( ) 를 이용해 어떤 연산자가 어떤 수식에 적용되는지 명확히 하는 것은 좋은 습관이다.
*, /, % 등의 연산자가 +, - 보다 우선 순위가 높다는 것 정도는 당연하게 전달될 수 있겠다.
그 외의 경우에는 (a + b) << c  처럼 적절히 ( ) 를 이용하는 것이 가독성을 높일 수 있다.


{:class="go-to-index"}
[Programming in Scala](index)