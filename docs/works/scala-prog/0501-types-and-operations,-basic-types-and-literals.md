# Types and Operations, Basic Types and Literals

2011-07-17 16:38

(이 장은 상식적인 내용이니 짧게 쓰고 넘어가기로 한다.)

### Some basic types / 기본 타입

	Byte, 8 bit signed
	Short, 16 bit signed
	Int, 32 bit signed
	Long, 64 bit signed
	Char, 16 bit unsigned unicode
	String, 스트링, Char 의 시퀀스
	Float, 32 bit float
	Double, 64 bit float
	Boolean, true or false

위 타입들은 scala 패키지에 정의되어 있다.
Int 는 scala.Int 다.
scala 패키지의 타입들은 모든 소스 파일에 임포트 되므로 Int 와 같은 짧은 이름을 쓸 수 있다.

스칼라의 기본 타입들은 자바의 원시 타입들과 같은 범위를 갖도록 정의되어 있다.
스칼라 컴파일러는 바이트코드를 생성할 때 스칼라 기본 타입들을 자바의 원시타입으로 변환한다.


### Literals / 리터럴

모든 스칼라 기본 타입은 리터럴 표기법을 가지고 있다.
리터널은 코드 내에 각 타입의 값을 기술하는 방법이다.

스칼라의 거의 모든 리터럴 문법은 자바와 같다.

	scala> val hex = 0x5
	hex: Int = 5
	scala> val hex2 = 0x00FF
	hex2: Int = 255
	scala> val magic = 0xcafebabe
	magic: Int = -889275714

	scala> val oct = 035  // (35 octal is 29 decimal)
	oct: Int = 29
	scala> val nov = 0777
	nov: Int = 511
	scala> val dec = 0321
	dec: Int = 209

	scala> val dec1 = 31
	dec1: Int = 31
	scala> val dec2 = 255
	dec2: Int = 255
	scala> val dec3 = 20
	dec3: Int = 20

	scala> val prog = 0XCAFEBABEL
	prog: Long = 3405691582
	scala> val tower = 35L
	tower: Long = 35
	scala> val of = 31l
	of: Long = 31

	scala> val little: Short = 367
	little: Short = 367
	scala> val littler: Byte = 38
	littler: Byte = 38

	scala> val big = 1.2345
	big: Double = 1.2345
	scala> val bigger = 1.2345e1
	bigger: Double = 12.345
	scala> val biggerStill = 123E45
	biggerStill: Double = 1.23E47

	scala> val little = 1.2345F
	little: Float = 1.2345
	scala> val littleBigger = 3e5f
	littleBigger: Float = 300000.0

	scala> val anotherDouble = 3e5
	anotherDouble: Double = 300000.0
	scala> val yetAnother = 3e5D
	yetAnother: Double = 300000.0

	scala> val bool = true
	bool: Boolean = true
	scala> val fool = false
	fool: Boolean = false

	scala> val a = 'A'
	a: Char = A

	scala> val c = '\101'
	c: Char = A

	scala> val d = '\u0041'
	d: Char = A
	scala> val f = '\u0044'
	f: Char = D

유니코드 캐릭터 표기법은 프로그램내 아무곳에나 올 수 있다.

	scala> val B\u0041\u0044 = 1
	BAD: Int = 1

캐릭터 리터럴 이스케이프 시퀀스

	\n line feed (\u000A)
	\b backspace (\u0008)
	\t tab (\u0009)
	\f form feed (\u000C)
	\r carriage return (\u000D)
	\" double quote (\u0022)
	\' single quote (\u0027)
	\\ backslash (\u005C)

	scala> val backslash = '\\'
	backslash: Char = \

	scala> val hello = "hello"
	hello: java.lang.String = hello

	scala> val escapes = "\\\"\'"
	escapes: java.lang.String = \"'

스칼라에는 """ 스트링 리터럴이 있다.

	println("""Welcome to Ultamix 3000.
			Type "HELP" for help.""")

결과

	Welcome to Ultamix 3000.
			Type "HELP" for help.

| 문자와 stripMargin 메서드로 각 행 앞머리의 공백을 삭제할 수 있다.
단, 보다시피 이건 컴파일 타임 작업이 아니고 런타임 작업임을 주의하자.

	println("""|Welcome to Ultamix 3000.
			|Type "HELP" for help.""".stripMargin)

	Welcome to Ultamix 3000.
	Type "HELP" for help.


### Symbol literals / 심볼 리터럴

심볼은 'ident 와 같이 쓴다. ident 자리에는 알파벳이나 숫자가 올 수 있다.
심볼 리터덜은 scala.Symbol 타입의 인스턴스로 매핑된다.
'symbal 은 Symbol("cymbal") 과 의미가 같다.

심볼로 할 수 있는 일은 이름을 얻는 것 외에 거의 없다.

	scala> val s = 'aSymbol
	s: Symbol = 'aSymbol
	scala> s.name
	res20: String = aSymbol

같은 심볼들 두번 쓰면 두 오브젝트는 같은 심볼 오브젝트를 가리킨다.