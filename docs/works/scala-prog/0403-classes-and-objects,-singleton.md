# Classes and Objects, Singleton

2011-07-15 02:36

### Singleton objects / 싱글턴 오브젝트

스칼라 클래스에는 static 멤버가 없다.
대신 싱글턴 오브젝트가 있다.

어떤 클래스에 대해 딱 한 개의 오브젝트만 만들어 쓸 때 이러한 방식을 싱글턴 패턴이라 한다.
싱글턴 패턴에서 생성되는 오브젝트가 싱글턴 오브젝트다. 줄여서 싱글턴이라고도 한다.

자바에서는 싱글턴을 생성하고 관리해 주는 getInstance 등의 static 펙토리 메서드를 사용해 구현하는데
스칼라에서는 문법적으로 싱글턴 패턴을 지원한다.

싱글턴의 정의는 클래스 정의와 비슷하다.
단지 class 키워드 대신 object 키워드를 사용한다.

	// In file ChecksumAccumulator.scala
	import scala.collection.mutable.Map

	object ChecksumAccumulator {
		private val cache = Map[String, Int]()

		def calculate(s: String): Int =
			if (cache.contains(s))
				cache(s)
			else {
				val acc = new ChecksumAccumulator
				for (c <- s)
					acc.add(c.toByte)
				val cs = acc.checksum()
				cache += (s -> cs)
				cs
			}
	}

여기서 싱글턴의 이름은 ChecksumAccumulator 이다.
대문자로 시작했지만 이것은 오브젝트다.

싱글턴의 정의에서 싱글턴의 타입은 사용자가 명시하지 않는다. 싱글턴의 이름만 명시한다.
싱글턴이 사용할 타입은 스칼라 컴파일러가 합성하고 싱글턴을 생성하고 관리하는 코드를 삽입한다.
(싱글턴의 이름이 ChecksumAccumulator 일 때 싱글턴을 위해 생성되는 클래스의 이름은 ChecksumAccumulator$ 다.)

특별한 것은 앞서 봤던 예에 등장하는 클래스의 이름과 싱글턴의 이름과 같다는 것이다.
싱글턴과 같은 이름의 클래스가 있을 경우 이들을 단짝 관계라고 한다.
싱글턴은 클래스의 단짝 오브젝트(companion object)가 되고
클래스는 싱글턴의 단짝 클래스(companion class)가 된다.

책의 예에서는 단짝 오브젝트 기능을 갖는 싱글턴을 보여주고 있는데
싱글턴이 꼭 어떤 클래스를 상정하고 단짝으로 존재해야할 필요는 없다.
단짝 클래스 없는 싱글턴도 가능하다.

(companion 을 뭐라고 번역해야 하나 고민하다가 앞으로 단짝이라고 일단 하겠다. 앞에선 모두 보조라고 적어놨는데. =,=)

서로 단짝 관계인 클래스와 싱글턴 오브젝트는 같은 파일에 정의해야한다.
단짝 클래스와 오브젝트는 서로의 비공개(private) 멤버에 접근할 수 있다.

싱글턴의 메서드는 아래와 같이 호출한다.

	ChecksumAccumulator.calculate("Every value is an object.")

싱글턴은 자바의 static 메서드에 비해 다른 클래스와 트레잇을 계승할 수 있다는 장점을 가진다.
자바의 static 방식으로는 코드를 계승할 수가 없다.
싱글턴은 슈퍼 클래스와 트레잇의 인스턴스이기 때문에 부모 타입들의 메서드를 호출할 수 있고
부모 타입들의 변수에 대입할 수 있고, 부모 타입을 인자로 받는 메서드에 전달할 수도 있다.
자동으로 생성되었을 뿐 이것은 일반 오브젝트와 다름 없는 오브젝트다.

클래스와 트레잇을 계승하는 싱글턴 오브젝트의 예는 13 장에서 볼 수 있다.

싱글턴은 생성 인자를 받을 수 없다.
new 키워드로 생성할 수 없기 때문이다.

JVM 레벨에서 보면 싱글턴 오브젝트는 합성된 클래스의 static 변수가 참조하는 인스턴스로 구현된다.
자바에서 싱글턴을 수작업으로 구현했을 때의 상태와 비슷하다.
초기화가 발생하는 시점도 자바 static 과 같다.

### 참고

처음 스칼라 싱글턴을 만났을 때 이해도 잘 안 되고 내부 구현에 대한 명확한 설명이 없어서 깝깝했던 기억이 있다.
그래서 이 섹션은 거의 내 맘대로 썼다. =o=
내용이 잘 전달되지 않았다면 그건 원전 저자들 책임이 아니고 내 책임이다. =,=
