---
title: Functional Objects, Part 3
---

2011-07-21 01:21

### Private fields and methods / 비공개 필드와 메서드

    scala> new Rational(66, 42)
    res6: Rational = 66/42

약분이 되지 않는다!
66/42 를 입력했을 때 11/7 이 되면 좋을 것이다.
약분은 분자와 분모를 최대 공약수(greatest common divisor)로 나누면 된다.

    class Rational(n: Int, d: Int) {
      require(d != 0)

      private val g = gcd(n.abs, d.abs)

      val numer = n / g
      val denom = d / g
      def this(n: Int) = this(n, 1)
      def add(that: Rational): Rational =
        new Rational(
          numer * that.denom + that.numer * denom,
          denom * that.denom
        )
      override def toString = numer +"/"+ denom

      private def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
    }

private 키워드로 시작하면 비공개 멤버를 정의할 수 있다.

이번 코드에서는 최대공약수를 담는 비공개 val g 를 추가하고 numer 와 denom 의 초기화 코드를 수정했다.
비공개 변수는 클래스 내부에서는 사용될 수 있으나 외부에서는 접근할 수 없다.

비공개 메서드 gcd 도 추가했다.
두 개의 정수를 전달 받고 최대공약수를 계산한다.

컴파일러는 세 개 필드의 초기화 코드를 보이는 순서대로 기본 생성자에 모은다.

    scala> new Rational(66, 42)
    res7: Rational = 11/7


### Defining operators / 오퍼레이터 정의하기

x + y 가 x.add(y) 나 x add y 보다 나아 보인다.
오퍼레이터 메서드를 추가해 보자.

    class Rational(n: Int, d: Int) {
      require(d != 0)
      private val g = gcd(n.abs, d.abs)
      val numer = n / g
      val denom = d / g
      def this(n: Int) = this(n, 1)

      def + (that: Rational): Rational = new Rational(numer * that.denom + that.numer * denom, denom * that.denom)
      def * (that: Rational): Rational = new Rational(numer * that.numer, denom * that.denom)

      override def toString = numer +"/"+ denom
      private def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
    }

\+ 와 * 메서드를 정의 했다.

    scala> val x = new Rational(1, 2)
    x: Rational = 1/2
    scala> val y = new Rational(2, 3)
    y: Rational = 2/3
    scala> x + y
    res8: Rational = 7/6


### Identifiers in Scala / 스칼라의 식별자

알파벳과 숫자로 구성된 식별자는 문자나 밑줄로 시작한다.
뒤에는 숫자도 붙을 수 있다.
$ 도 문자로 간주되지만 스칼라 컴파일러가 생성하는 식별자를 위해 예약되어 있다.
컴파일 오류는 나지 않지만 사용자는 $ 를 사용하면 안 된다.

스칼라는 자바의 낙타 표기법(camelCase)을 사용한다.
밑줄 _ 도 가능은 하지만 자주 사용되지는 않는다.
밑줄은 스칼라에서 다른 여러가지 용도로 사용되니 가급적 식별자에 사용을 피하라.

펑션은 소문자로 시작, 클래스와 트레잇은 대문자로 시작.

상수 이름에서 스칼라와 자바에 차이가 있다.
val 은 초기화 된 후 값이 변하지 않기 때문에 상수 처럼 보이지만 이것은 엄연히 변수다.
메서드 인자도 val 변수다. 메서드 인자의 val 값은 메서드가 호출될 때마다 다른 값을 갖는다.

자바에서는 상수를 MAX_VALUE, X_OFFSET 처럼 쓴다.
스칼라에서는 MaxValue, XOffset 처럼 쓴다.

(
스칼라는 자바의 상수와 같은 것은 만들 수 없다.
자바 상수에 가장 가까운 것은 val 변수다.
val 은 값이 변하진 않지만 변수다.
val 은 낙타 표기법을 쓰는데 상수 역할을 한다면 첫 문자를 대문자로 쓴다.
)

오퍼레이터 식별자는 한 개 이상의 오퍼레이터 문자를 쓴다.

예) +, :, ?, ~, #, +, ++, :::, <?>, :->

스칼라 컴파일러는 오퍼레이터 식별자를 자바 식별자 규칙에 맞도록 변환한다.

예) :-> 은 $colon$minus$greater 가 된다.

x<-y 를 자바에서 파싱하면 x < - y 가 되지만 스칼라에서는 <- 가 붙어서 x <- y 가 된다.
스칼라에서는 특수기호가 메서드 명이 될 수 있으므로 코딩할 때는 적절한 공백으로 식별자끼리 구분한다.

스칼라 키워드를 식별자로 쓰려면 백틱(back tick, grave accent, `)으로 감싼다.
yield 는 스칼라의 키워드이기 때문에 Thread.yield() 대로 호출할 수 없다
Thread.`yield`() 로 적는다.


### Method overloading / 메서드 오버로딩

유리수와 정수간의 연산을 만들어 보자.
유리수간 * 메서드가 이미 있으므로 정수를 위한 * 메서드를 추가한다.

(오버로딩에 관한 상식적인 설명 생량. =,=)

    class Rational(n: Int, d: Int) {
      require(d != 0)
      private val g = gcd(n.abs, d.abs)
      val numer = n / g
      val denom = d / g

      def this(n: Int) = this (n, 1)

      def +(that: Rational): Rational =
        new Rational(
          numer * that.denom + that.numer * denom,
          denom * that.denom
        )

      def +(i: Int): Rational =
        new Rational(numer + i * denom, denom)

      def -(that: Rational): Rational =
        new Rational(
          numer * that.denom - that.numer * denom,
          denom * that.denom
        )

      def -(i: Int): Rational =
        new Rational(numer - i * denom, denom)

      def *(that: Rational): Rational =
        new Rational(numer * that.numer, denom * that.denom)

      def *(i: Int): Rational =
        new Rational(numer * i, denom)

      def /(that: Rational): Rational =
        new Rational(numer * that.denom, denom * that.numer)

      def /(i: Int): Rational =
        new Rational(numer, denom * i)

      override def toString = numer + "/" + denom

      private def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
    }

실행

    scala> val x = new Rational(2, 3)
    x: Rational = 2/3
    scala> x * x
    res13: Rational = 4/9
    scala> x * 2
    res14: Rational = 4/3


{:class="go-to-index"}
[Programming in Scala](index)
