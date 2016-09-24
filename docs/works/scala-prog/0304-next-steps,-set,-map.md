---
title: First Steps, Set, Map
---

2011-07-12 23:42

### Use sets and maps / 셋, 맵 사용하기

스칼라는 F 스타일과 절차적 스타일의 장점을 모두 수용하는 것이 목표이기 때문에
컬렉션 라이브러리에서 불변 컬렉션과 가변 컬렉션의 구분을 확실히 하고있다.

어레이는 항상 가변이다.
리스트는 항상 불변이다.

셋과 맵은 가변과 불변 타입을 모두 제공한다.
이들은 패키지 네임스페이스와 클래스 계승 관계는 다르고 클래스 이름은 같다.

[p92 Figure 3.2 Class hierarchy for Scala sets] 참고

Set 에 관한 베이스 트레잇이 있다.

    scala.collection.Set.

그리고 불변, 가변 셋을 위해 각각의 서브 트레잇이 있다.

    scala.collection.immutable.Set,
    scala.collection.mutable.Set.

세 개의 트레잇 이름은 Set 으로 모두 같고 패키지만 다르다.

위 트레잇들을 계승하는 구체적 클래스들이 있다. 아래 예는 HashSet.

    scala.collection.immutable.HashSet,
    scala.collection.mutable.HashSet.

당장은 트레잇을 자바의 인터페이스와 비슷하다 생각하고 넘어가도 좋다.

셋 사용법은 아래와 같다.

    var jetSet = Set("Boeing", "Airbus")
    jetSet += "Lear"
    println(jetSet.contains("Cessna"))

결과

    false

불변 Set 클래스는 기본으로 임포트된다.
리스트와 어레이의 경우처럼 Set 도 Set 보조 오브젝트의 apply() 펙토리 메서드를 사용해 생성하고 있다.
여기서 사용된 Set 은 scala.collection.immutable.Set 보조 오브젝트다.

셋에 원소를 추가하기 위해서는 추가할 원소를 인자로 셋의 + 메서드를 호출한다.
가변, 불변 셋 모두 + 메서드를 제공한다.
가변 셋은 새로운 원소를 자기 자신에게 추가하고, 불변 셋은 원소가 추가된 새로운 셋을 생성해 리턴한다.
위 예에서는 불변 셋이 사용되고 있으므로 여기서 + 연산은 완전히 새로운 셋을 리턴한다.

가변 셋에는 += 메서드의 정의가 따로 되어 있지만 불변 셋에는 없다.
그래서 jetSet += "Lear" 는 jetSet = jsetSet + "Lear" 으로 풀어서 해석한다.
var jetSet 을 val jetSet 으로 정의했으면 재대입이 불가능하므로 컴파일 에러가 난다.

가변 셋을 짧은 이름으로 사용하려면 아래와 같이 import 구문을 사용해야 한다.

    import scala.collection.mutable.Set

    val movieSet = Set("Hitch", "Poltergeist")
    movieSet += "Shrek"
    println(movieSet)

자바에서 처럼 import 를 하게 되면 긴 이름을 사용하지 않고 간단한 이름을 사용할 수 있다.
여기에서의 Set 은 scala.collection.mutable.Set 보조 오브젝트를 의미하게 된다.
mutable.Set 의 apply 메서드는 가변 셋 오브젝트를 리턴한다.

가변 셋에는 += 메서드가 정의되어 있다.
그래서 movieSet += "Shrek" 은 movie.+=("Shrek") 가 된다.
movieSet += 에서는 재대입이 발생하지 않으므로 val movieSet 이 될 수 있다.

대부분의 경우 위에 설명한 Set 오브젝트 용법으로 충분하겠지만
간혹 사용할 셋 클래스를 명시적으로 적을 필요가 있을 수 있다.

다행히 문법은 아래와 같이 비슷하다.

    import scala.collection.immutable.HashSet

    val hashSet = HashSet("Tomatoes", "Chilies")
    println(hashSet + "Coriander")

(
일반적인 경우 HashSet 오브젝트 대신 Set 오브젝트로 셋을 생성하는 것이 유리하다.
Set 오브젝트는 셋의 원소가 4 개 이하인 경우 원소 갯수별로 특별히 제작된 클래스를 사용하므로 최적화된다.
원소가 5 개 이상이 되면 HashSet 을 사용하기 시작한다.
)

스칼라는 또 다른 유용한 컬렉션인 Map 도 제공한다.
Map 도 Set 과 마찬가지로 불변, 가변 버전이 있다.

[p94 Figure 3.3 Class hierarchy for Scala maps] 참고

    import scala.collection.mutable.Map

    val treasureMap = Map\[Int, String\]()
    treasureMap += (1 -> "Go to island.")
    treasureMap += (2 -> "Find big X on ground.")
    treasureMap += (3 -> "Dig.")
    println(treasureMap(2))

결과

    Find big X on ground.

가변 셋 예제 때와 비슷하게 가변 클래스를 임포트 해서 val 오브젝트 treasureMap 을 생성했다.
빈 맵이라 타입 추론을 할 수 없으므로 타입 정보가 명시되었다.
키 타입은 Int 이고 값 타입은 String 이다.

1 -> "Go to island." 구문도 역시나 특별한 것은 없다.
(1).->("Go to island.") 메서드 호출이다.

-> 메서드는 어떤 스칼라 오브젝트에도 쓸 수 있는데 원소가 두 개인 튜플을 리턴한다.
즉, (1, "Go to island.") 과 효과는 같다.
맵 인자를 만드는 등의 경우에 코드를 보기 좋게 해준다.

불변 맵은 기본으로 임포트되기 때문에 별도 import 구문 없이 사용할 수 있다.

    val romanNumeral = Map(1 -> "I", 2 -> "II", 3 -> "III", 4 -> "IV", 5 -> "V")
    println(romanNumeral(4))

결과

    IV

임포트 없이 사용한 Map 은 scala.collection.immutable.Map 이 된다.
다섯 개의 키/값 튜플들을 팩토리 메서드에 전달하고 있다.
구체적인 초기화 인자를 전달하는 경우 별도 타입 정보를 적을 필요는 없다.


{:class="go-to-index"}
[Programming in Scala](index)
