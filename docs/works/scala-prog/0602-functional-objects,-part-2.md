# Functional Objects, Part 2

2011-07-20 00:19

### Adding fields / 필드 추가

덧셈 기능을 추가해 보자.
이를 위해서 다른 유리수 오브젝트를 인자로 받는 add 메서드를 추가할 것이다.
오브젝트를 불변상태로 유지하기 위해 add 는 인자로 받은 유리수를 자신에게 더해서는 안 된다.
더한 값을 갖는 새로운 유리수를 만들어서 리턴할 것이다.
add 메서드를 아래와 같이 만들 수 있다고 생각할지 모르겠다.

	class Rational(n: Int, d: Int) { // This won’t compile
		require(d != 0)
		override def toString = n +"/"+ d
		def add(that: Rational): Rational =
			new Rational(n * that.d + that.n * d, d * that.d)
	}

하지만 아래와 같은 컴파일 오류가 발생한다.

	<console>:11: error: value d is not a member of Rational
		new Rational(n * that.d + that.n * d, d * that.d)

	<console>:11: error: value d is not a member of Rational
		new Rational(n * that.d + that.n * d, d * that.d)

(
웹 페이지라 정확한 오류위치 마크 표시가 어려운데, 첫번 째 인자의 that.d 와 두번 째 인자의 that.d 에서 에러가 났다.
첫번째 인자에서 that.d 가 없었다면 that.n 에서 에러가 났을 것이다.
한 수식에서 먼저 에러난 곳만 잡는 듯.
)

add 메서드는 클래스 인자 n 과 d 를 사용할 수 있지만 메서드가 적용된 오브젝트의 n 과 d 만 사용 가능하다.
that 의 n 과 d 는 add 메서드가 적용된 오브젝트의 것이 아니므로 쓸 수 없다.
that 의 n 과 d 를 사용하려면 이들을 필드로 만들어야 한다.

(
기본적으로 클래스 인자의 목적은 오브젝트 초기화다.
오브젝트 초기화 이후 외부로 속성을 보이는 기능은 일반 필드가 해야할 일이다.
클래스 인자 n 과 d 가 사용되는 방식에 따라 내부적으로 보이지 않는 필드가 사용될 수는 있다.
하지만 그런 필드가 생성되더라도 스칼라가 내보이진 않는다.
필드가 필요하면 명시적으로 필드 정의를 해야한다.
)

	class Rational(n: Int, d: Int) {
		require(d != 0)

		val numer: Int = n
		val denom: Int = d

		override def toString = numer +"/"+ denom
		def add(that: Rational): Rational =
			new Rational(
				numer * that.denom + that.numer * denom,
				denom * that.denom
			)
	}

numer, denum 두 필드를 추가하고 클래스 인자로 초기화했다.
toString 과 add 메서드를 필드를 사용하도록 수정했다.

(
클래스 인자, 필드, 두 벌 나오는 것이 좀 번잡해 보이는데 참자.
10.6 절에서 클래스 인자와 필드 정의를 한방에 축약해서 적는 방법을 소개한다.
)

사용 예는 아래와 같다.

	scala> val oneHalf = new Rational(1, 2)
	oneHalf: Rational = 1/2
	scala> val twoThirds = new Rational(2, 3)
	twoThirds: Rational = 2/3
	scala> oneHalf add twoThirds
	res3: Rational = 7/6

필드를 쓸 수도 있다.

	scala> val r = new Rational(1, 2)
	r: Rational = 1/2
	scala> r.numer
	res4: Int = 1
	scala> r.denom
	res5: Int = 2


### Self references / 자기 참조

this 키워드는 현재 실행되고 있는 메서드나 생성자의 대상이 되는 오브젝트를 가리킨다.

def lessThan(that: Rational) = this.numer * that.denom < that.numer * this.denom

위 메서드는 lessThan 을 적용한 오브젝트가 that 보다 작은지를 판별한다.
위 수식에서는 this 를 생략해도 의미는 같다.

아래는 this 가 꼭 필요한 예다.
앞의 this 는 없어도 되지만 마지막 this 는 빠지면 망한다.

def max(that: Rational) = if (this.lessThan(that)) that else this


### Auxiliary constructors / 보조 생성자

종종 여러 개의 생성자가 필요할 수 있다.
기본 생성자(primary constructor)외 기타 생성자들을 보조 생성자(auxiliary constructor)라고 한다.

분모가 1 일때 분자만 받는 유리수 생성자를 만들면 더 간단해질 것이다.
new Rational(5, 1) 대신에 new Rational(5) 과 같이 쓸 수 있다.

	class Rational(n: Int, d: Int) {
		require(d != 0)
		val numer: Int = n
		val denom: Int = d

		def this(n: Int) = this(n, 1) // auxiliary constructor

		override def toString = numer +"/"+ denom
		def add(that: Rational): Rational =
			new Rational(
				numer * that.denom + that.numer * denom,
				denom * that.denom
			)
	}

보조 생성자는 def this(...) 로 정의한다.
예에서 보조 생성자는 기본 생성자를 호출하고 있다.

	scala> val y = new Rational(3)
	y: Rational = 3/1

모든 보조 생성자는 다른 모든 작업에 앞서 같은 클래스의 다른 생성자를 호출해야 한다.
즉 모든 보조 생성자의 첫 문장에는 this(...) 가 온다.
호출되는 생성자는 기본 생성자일 수도 있고 먼저 정의된 다른 보조 생성자일 수도 있다.
결국 기본 생성자는 어떤 경로를 통해서라도 호출되고 제일 처음 실행된다.

자바에서 생성자는 다른 생성자를 호출할 수도 있고 슈퍼 클래스의 생성자를 바로 호출할 수도 있다.
스칼라에서는 기본 생성자만이 슈퍼 클래스의 생성자를 호출할 수 있다.
스칼라 방식이 좀더 제한적인데 생성자를 간단하게 만들기 위한 문법을 만들면서 생긴 다소간의 단점이다.

계승과 슈퍼클래스에 대해서는 10 장에서 다룬다.