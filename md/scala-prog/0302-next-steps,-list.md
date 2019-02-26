---
title: First Steps, List
---

2011-07-12 01:28

### Use List / 리스트 사용하기

메서드에 부수효과(side effect)가 없어야 한다는 것은 FP 의 중요한 개념중 하나다.
메서드의 유일한 행동은 값을 계산하고 리턴하는 것이어야 한다.
입력이 같으면 결과는 항상 같아야 한다.
이 방식에 따라 메서드를 만들면 메서드의 안정성이 높아지고 재사용성도 증가한다.

이 F 철학을 오브젝트 세계에 적용하면 불변(immutable) 오브젝트 개념이 등장한다.
이전 예제에서 보았듯이 스칼라 Array 는 가변(mutable) 오브젝트다.
어레이는 생성한 후 길이를 바꿀 수는 없지만 원소들은 바꿀 수 있다.

어레이와 반대로 스칼라 List 는 동일 타입을 같는 오브젝트들의 불변 시퀀스다.
Array[String] 과 비슷하게 List[String] 은 스트링만 가질 수 있다.
스칼라 List, 즉 scala.List 는 자바의 Java.util.List 와 다른 것이다.
자바의 List 는 가변이지만 스칼라의 List 는 불변이다.
스칼라의 List 는 F 스타일로 프로그래밍할 수 있도록 만들어졌다.

리스트를 생성하는 법은 쉽다.

    val oneTwoThree = List(1, 2, 3)

oneTwoThree val 은 1, 2, 3 원소로 초기화된 List[Int] 타입 오브젝트다.
List.apply() 펑션이 준비되어 있어서 new List 로 오브젝트를 생성하고 별도로 초기화하는 번거로운 과정은 필요없다.
리스트는 변경불가능하므로 자바 스트링과 비슷한 용법을 보인다.
먼가 내용물을 바꾸는 듯한 뉘앙스를 가진 리스트 메서드들은 새로 생성된 리스트를 리턴한다.

리스트들을 결합하는 ':::' 이란 이름의 메서드가 있다.

    val oneTwo = List(1, 2)
    val threeFour = List(3, 4)
    val oneTwoThreeFour = oneTwo ::: threeFour
    println(oneTwo + " and " + threeFour + " were not mutated.")
    println("Thus, " + oneTwoThreeFour + " is a new list.")

실행 결과는

    List(1, 2) and List(3, 4) were not mutated.
    Thus, List(1, 2, 3, 4) is a new list.

또 다른 메서드로 :: 이 있다. 이것은 cons 라고 읽는다. (리스프 문화로부터의 영향)
cons 는 리스트 앞에 새로운 원소를 연결한 리스트를 리턴한다.

    val twoThree = List(2, 3)
    val oneTwoThree = 1 :: twoThree
    println(oneTwoThree)

결과는

    List(1, 2, 3)

1 :: twoThree 에서 :: 는 1 에 적용되는 메서드가 아니라twoThree 의 메서드다. 깜놀?
스칼라의 독특한 규칙이 또 하나 등장한다.
메서드 이름이 : 으로 끝나면 해당 메서드는 오른쪽 오브젝트에 대해서 실행된다.
그러므로 1 :: twoThree 는 (1).::(twoThreee) 가 아니라 twoThree.::(1) 이다.

List(1, 2, 3) 을 다음과 같이 만들 수도 있다.

    val oneTwoThree = 1 :: 2 :: 3 :: Nil
    println(oneTwoThree)

여기서 Nil 은 빈 리스트 오브젝트다.
Nil 을 맨 뒤에 두는 이유는 :: 메서드가 리스트 오브젝트에 정의되어 있기 때문이다.
:: 메서드가 Int 클래스에는 없기 때문에 1 :: 2 :: 3 이라고 쓸 수 없다.

리스트의 뒤에 원소를 추가하는 메서드도 있긴 한데 잘 사용되지는 않는다.
리스트 뒤에 원소를 붙이려면 기존 리스트 전체를 복제해야 하기 때문이다.
원소를 앞에 붙일 때는 이런 과정이 필요 없다.
기존의 리스트는 불변이라 가정할 수 있기 때문에
앞에 추가되는 원소를 담을 노드만 새로 만들어서 여기에 기존 리스트를 연결하면 된다.

다음은 리스트에 관한 여러가지 수식과 설명

List() or Nil : 빈 리스트

List("Cool", "tools", "rule") : 세 스트링을 갖는 List[String]

val thrill = "Will" :: "fill" :: "until" :: Nil : 세 스트링을 갖는 List[String]

List("a", "b") ::: List("c", "d") : 두 리스트를 연결, List("a", "b", "c", "d")

thrill(2) : thrill 리스트의 인덱스 2 원소, "until"

thrill.count(s => s.length == 4) : 길이가 4 인 thrill 리스트의 원소 갯수, 2

thrill.drop(2) : 앞 두 원소를 버림, List("until")

thrill.dropRight(2) : 마지막 두 원소를 버림, List("Will")

thrill.exists(s => s == "until") : "until" 이란 원소가 있는지 검사, true

thrill.filter(s => s.length == 4) : 길이가 4 인 원소들의 리스트, List("Will", "fill")

thrill.forall(s => s.endsWith("l")) : 모든 원소의 끝이 "l" 로 끝나는지 확인, true

thrill.foreach(s => print(s)) : 모든 원소들을 출력 "Willfilluntil"

thrill.foreach(print) : 위와 동일, 좀 더 간단

thrill.head : 첫 원소, "Will"

thrill.init : 마지막 원소를 제외, List("Will", "fill")

thrill.isEmpty : 리스트가 비었나?, false

thrill.last : 마지막 원소, "until"

thrill.length : 리스트의 길이, 3

thrill.map(s => s + "y") : 모든 원소끝에 "y" 를 붙인 리스트 생성, List("Willy", "filly", "untily")

thrill.mkString(", ") : 원소들을 연결해서 스트링 생성, "Will, fill, until"

thrill.remove(s => s.length == 4) : 길이가 4 인 원소를 제외, List("until")

thrill.reverse : 순서 역전, List("until", "fill", "Will")

thrill.sort((s, t) => s.charAt(0).toLower < t.charAt(0).toLower) : 첫 문자의 소문자 기준으로 소트, List("fill", "until", "Will")

thrill.tail : 첫 원소 제외, List("fill", "until")


### 참고

List 는 너무 자주 사용되기 때문에 무려 scala 패키지에 올라와 있다.
사실 List 포함 다른 모든 컬렉션 라이브러리는 scala.collection 패키지에 있다.
scala 패키지에 들어있는 List 클래스와 List 보조 오브젝트는 scala.collection.immutable.List 의 앨리어스다.

<http://lampsvn.epfl.ch/trac/scala/browser/scala/trunk/src/library/scala/package.scala>{:target="_blank"}

    type List[+A] = scala.collection.immutable.List[A]
    val List = scala.collection.immutable.List
    val Nil = scala.collection.immutable.Nil


{:class="go-to-index"}
[Programming in Scala](index)
