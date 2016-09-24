---
title: Types and Operations, Operators
---

2011-07-17 21:35


### Operators are methods / 연산자는 메서드다

스칼라는 기본 타입을 위해 충분한 연산자들을 제공한다.
연산자는 실제로 메서드 콜로 구현된다.

Int 클래스는 Int 를 인자로 받아서 Int 를 리턴하는 + 메서드를 가지고 있다.
이 + 메서드는 두개의 Int 를 더할 때 호출된다.

1 + 2 는 (1).+(2) 와 같다.

    scala> val sum = 1 + 2    // Scala invokes (1).+(2)
    sum: Int = 3

    scala> val sumMore = (1).+(2)
    sumMore: Int = 3

Int 는 다른 다양한 타입들을 인자로 받는 + 메서드들도 가지고 있다.

    scala> val longSum = 1 + 2L    // Scala invokes (1).+(2L)
    longSum: Long = 3

연산자 표기법은 연산자처럼 보이지 않는 메서드에도 사용할 수 있다.
스트링의 indexOf 메서드는 Char 인자를 받아서 문자가 처음 발견되는 위치를 리턴한다.

    scala> val s = "Hello, world!"
    s: java.lang.String = Hello, world!
    scala> s indexOf 'o'     // Scala invokes s.indexOf(’o’)
    res0: Int = 4

스트링에는 검색할 문자와 검색을 시작할 위치, 두 개의 인자를 받는 indexOf 도 있다.
인자를 여러 개 받는 메서드를 연산자 표기법으로 호출할 때는 인자를 ( ) 로 감싸야 한다.

    scala> s indexOf ('o', 5) // Scala invokes s.indexOf(’o’, 5)
    res1: Int = 8

연산자는 특별한 문법이 아니다.
어떤 메서드라도 연산자가 될 수 있다.
어떻게 쓰느냐에 따라 메서드는 연산자가 될 수 있다.
s.indexOf('o') 와 같이 적을 때 indexOf 는 연산자가 아니지만,
s indexOf 'o' 와 연산자 표기법으로 적을 때 indexOf 는 연산자다.

이항 연산자 뿐만 아니라 전치, 후치 연산자에 대한 연산자 표기법도 있다.
전치 표기란 메서드 이름을 오브젝트 앞에 두는 것이다. ex) -7, -2.0, !found, ~0xFF
후치 표기란 메서드 이름을 오브젝트 뒤에 두는 거이다. ex) 7 toLong

전치 연산자도 메서드 콜에 대한 약식 표기다.
단 이때는 메서드 이름 앞에 "unary_" 가 붙는다.
-2.0 은 (2.0).unary_- 가 된다.

    scala> -2.0
    res2: Double = -2.0
    scala> (2.0).unary_-
    res3: Double = -2.0

전치 연산자로 쓰일 수 있는 식별자는 +, -, ~, ~ 뿐이다.
unary_* 같은 메서드를 정의해도 전치 연산자로 사용하지 못한다.
*p 와 같은 구분은 *.p 로 파싱된다.

후치 연산자는 인자를 받지 않는 메서드를 . 이나 ( ) 없이 호출하는 것이다.
스칼라에서는 메서드 콜에 사용되는 빈 ( ) 를 생략할 수 있다.
println() 처럼 메서드가 부수효과를 가질 경우에는 ( ) 를 적어주는 것이 관례다.
toLowerCase 처럼 부수효과가 없을 경우에는 ( ) 를 삭제해도 좋다.

    scala> val s = "Hello, world!"
    s: java.lang.String = Hello, world!
    scala> s.toLowerCase
    res4: java.lang.String = hello, world!
    scala> s toLowerCase
    res5: java.lang.String = hello, world!


이제부터 스칼라가 제공하는 기본 연산자들을 볼 것인데 거의 자바와 같다.

    scala> 1.2 + 2.3
    res6: Double = 3.5
    scala> 3 - 1
    res7: Int = 2
    scala> 'b' - 'a'
    res8: Int = 1
    scala> 2L * 3L
    res9: Long = 6
    scala> 11 / 4
    res10: Int = 2
    scala> 11 % 4
    res11: Int = 3
    scala> 11.0f / 4.0f
    res12: Float = 2.75
    scala> 11.0 % 4.0
    res13: Double = 3.0

    scala> 1 > 2
    res16: Boolean = false
    scala> 1 < 2
    res17: Boolean = true
    scala> 1.0 <= 1.0
    res18: Boolean = true
    scala> 3.5f >= 3.6f
    res19: Boolean = false
    scala> 'a' >= 'A'
    res20: Boolean = true
    scala> val thisIsBoring = !true
    thisIsBoring: Boolean = false
    scala> !thisIsBoring
    res21: Boolean = true

    scala> val toBe = true
    toBe: Boolean = true
    scala> val question = toBe || !toBe
    question: Boolean = true
    scala> val paradox = toBe && !toBe
    paradox: Boolean = false

    scala> def salt() = { println("salt"); false }
    salt: ()Boolean
    scala> def pepper() = { println("pepper"); true }
    pepper: ()Boolean
    scala> pepper() && salt()
    pepper
    salt
    res22: Boolean = false
    scala> salt() && pepper()
    salt
    res23: Boolean = false

모든 인자는 메서드가 실행되기 전에 값이 결정되어야 한다.
연산자가 메서드로 구현된다면 위와 같은 두번 째 피연산자 연산 생략이 어떻게 가능한 것일까?

스칼라는 인자 값의 평가를 연기시키거나 거부할 수 있는 기능이 있다.
이에 대해서는 9.5 절에서 설명한다.

    scala> 1 & 2
    res24: Int = 0
    scala> 1 | 2
    res25: Int = 3
    scala> 1 ˆ 3  // bitwise xors 0001 and 0011, yielding 0010
    res26: Int = 2
    scala> ~1   // invert 0001, yielding -2 (11111111111111111111111111111110)
    res27: Int = -2

    scala> -1 >> 31
    res28: Int = -1
    scala> -1 >>> 31
    res29: Int = 1
    scala> 1 << 2
    res30: Int = 4
    

{:class="go-to-index"}
[Programming in Scala](index)
