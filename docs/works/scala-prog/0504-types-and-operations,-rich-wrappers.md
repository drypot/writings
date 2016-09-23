# Types and Operations, Rich Wrappers

2011-07-17 23:08


### Rich wrappers / 싸개

기본 타입에 대해 호출할 수 있는 메서드는 위에 설명한 것들 보다 훨씬 많다.

	0 max 5 -> 5
	0 min 5 -> 0
	-2.7 abs -> 2.7
	-2.7 round -> -3L
	1.5 isInfinity -> false
	(1.0 / 0) isInfinity -> true
	4 to 6 -> Range(4, 5, 6)
	"bob" capitalize -> "Bob"
	"robert" drop 2 -> "bert"

이 메서드들은 자동 변환 (implicit conversions) 을 통해 제공된다.
기본 타입들은 필요에 따라 리치 타입으로 자동 변환 되므로 리치 타입의 메서드를 사용할 수 있다.
자동 변환에 대한 자세한 내용은 21 장에서 설명된다.

기본 타입들에 대해서 싸개 (rich wrapper) 들은 다음과 같다.

	Byte <---> scala.runtime.RichByte
	Short <---> scala.runtime.RichShort
	Int <---> scala.runtime.RichInt
	Char <---> scala.runtime.RichChar
	Float <---> scala.runtime.RichFloat
	Double <---> scala.runtime.RichDouble
	Boolean <---> scala.runtime.RichBoolean
	String <---> scala.collection.immutable.StringOps