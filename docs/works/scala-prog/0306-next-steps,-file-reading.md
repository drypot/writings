# First Steps, File Reading

2011-07-14 00:41

### Read lines from a file / 파일 읽기

파일을 한 줄 한 줄을 읽어들이는 작업은 일상 스크립트에서 비일비재하다.
이번 섹션에서는 파일을 한 줄 한 줄 읽어서 행의 길이와 내용을 출력하는 스크립트를 만들어 보겠다.
첫 버전은 아래와 같다.

	import scala.io.Source

	if (args.length > 0) {
		for (line <- Source.fromFile(args(0)).getLines())
			println(line.length +" "+ line)
	}
	else
		Console.err.println("Please enter filename")

코드를 보면 scala.io.Source 클래스를 임포트하고 있다.
쉘에서 인자가 입력됐는지 확인한다.
입력되었다면 첫번 째 인자를 입력 파일명으로 사용한다.
Source.fromFile(args(0)) 메서드는 Source 오브젝트를 리턴한다.
getLines 의 리턴 타입은 Iterator[String] 이다.
for 수식으로 파일의 각 행에 대해 println 구문을 적용한다.

(
println 은 Console.println 의 딴이름인데 scala.Predef 오브젝트에 정의되어 있다.
Console.err.println 은 딴이름이 정의되어있지 않으니 긴 이름을 써야한다.
)

스크립트를 저장하고 실행한 결과는 아래와 같다.

	$ scala countchars1.scala countchars1.scala
	22 import scala.io.Source
	0
	22 if (args.length > 0) {
	0
	51   for (line <- Source.fromFile(args(0)).getLines())
	35     println(line.length +" "+ line)
	1 }
	4 else
	46   Console.err.println("Please enter filename")

행 길이가 전혀 정렬되지 않았다!
아래와 같은 결과를 보고 싶다.

	22 | import scala.io.Source
	 0 |
	22 | if (args.length > 0) {
	 0 |
	51 |   for (line <- Source.fromFile(args(0)).getLines())
	35 |     println(line.length +" "+ line)
	 1 | }
	 4 | else
	46 |   Console.err.println("Please enter filename")

이렇게 하려면 행들을 두번 돌아야한다.
첫번 째 반복에서 행 길이 표시에 필요한 최대 폭을 결정하고,
두번 째 반복에서 파일 내용을 출력한다.

행들을 두번 반복할 것이니 변수에 담아두도록 한다.

	val lines = Source.fromFile(args(0)).getLines().toList

getLines() 는 반복자(iterator)를 리턴하기 때문에 마지막 toList 메서드가 필요하다.
반복자는 한번 사용하면 소진되어 재사용할 수가 없다.
반복자를 List 로 바꿔둠으로써 필요한 만큼 재사용할 수가 있다.
리스트를 사용하면 모든 파일 내용을 메모리에 한꺼번에 올려두는 비용이 들긴 한다.

아래는 스트링 길이를 표시하는데 필요한 문자 폭을 계산하는 수식이다.
별도 펑션으로 뽑아 봤다.

	def widthOfLength(s: String) = s.length.toString.length

이 함수를 가지고 폭의 최대값을 구하는 수식은 아래와 같다.

	var maxWidth = 0
	for (line <- lines)
		maxWidth = maxWidth.max(widthOfLength(line))

	var 를 사용하지 않으려면 일단 아래 수식으로 제일 긴 행을 구한다.

	val longestLine = lines.reduceLeft(
	  (a, b) => if (a.length > b.length) a else b
	)

reduceLeft 메서드는 인자로 받은 펑션을 컬력션의 첫 두 원소에 대해 적용하고
이 결과와 다음 원소에 다시 펑션을 적용하는 과정을 반복한다.

예에서 전달한 펑션은 입력받은 두 스트링 중 더 긴 스트링을 리턴한다.
이 펑션을 모든 lines 인자에 적용하면 가장 긴 스트링이 결과로 리턴된다.

lines 의 원소 타입이 String 이기 때문에 lines 의 reduceLeft 에 사용되는 a, b 의 타입은 String 으로 추론할 수 있다.

longestLine 에 widthOfLength 펑션을 적용하면 가장 긴 행의 길이를 구할 수 있다.
(F 적으로 코딩하면 이렇게 메서드를 재사용할 가능성이 늘어난다.)

	val maxWidth = widthOfLength(longestLine)

출력 구문은 아래와 같다.

	for (line <- lines) {
		val numSpaces = maxWidth - widthOfLength(line)
		val padding = " " * numSpaces
		println(padding + line.length + " | " + line)
	}

최종 결과 코드는 아래와 같다.

	import scala.io.Source

	def widthOfLength(s: String) = s.length.toString.length

	if (args.length > 0) {
		val lines = Source.fromFile(args(0)).getLines().toList
		val longestLine = lines.reduceLeft(
			(a, b) => if (a.length > b.length) a else b
		)
		val maxWidth = widthOfLength(longestLine)
		for (line <- lines) {
			val numSpaces = maxWidth - widthOfLength(line)
			val padding = " " * numSpaces
			println(padding + line.length +" | "+ line)
		}
	}
	else
		Console.err.println("Please enter filename")


### 참고

String 에 대한 + 연산이 발생하면 컴파일러가 StringBuffer 를 만들어 결합한다.
StringBuffer 수작업할 필요는 없다.

### 참고

F 스타일에서는 한 변수의 값을 바꾸지는 않지만 변하지 않는 더 많은 변수를 생성할 수 있다.
이 변수들은 펑션 호출 과정에서 스택에 생성되는 펑션 인자들이다.
경우에 따라 감당 못할 정도의 인자들이 스택에 쌓일 수도 있는데
이 문제를 해결하기 위해 FP 에는 tail recursion 이라는 해법을 쓴다.
문제는 Java 6 까지 JVM 한계로 효과적인 tail recursion 이 어려웠다.
Java 7 의 JVM 부터 tail recursion 지원이 들어간다.
그래서 스칼라 라이브러리는 스펙상 F 적이지만 내부 구현은 절차적으로 되어있다.
순수 F 언어들의 라이브러리는 tail recursion 을 사용해 만들어져있다.
