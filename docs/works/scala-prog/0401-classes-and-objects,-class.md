# Classes and Objects, Class

2011-07-14 20:39


### Classes, fields, and methods

클래스는 오브젝트를 위한 청사진이다.
클래스를 정의하면 new 키워드로 오브젝트를 생성할 수 있다.
아래는 클래스 정의 예다.

	class ChecksumAccumulator {
		// class definition goes here
	}

오브젝트는 다음과 같이 생성한다.

	new ChecksumAccumulator

클래스 정의 안에는 필드와 메서드를 둔다.
필드와 메서드를 합하여 멤버라고 한다.

필드는 변수이고 val 이나 var 를 가지고 정의한다.
메서드는 실행 코드이고 def 를 가지고 정의한다.
모든 인스턴스는 자기만의 변수를 갖기 때문에 필드를 인스턴스 변수라 부르기도 한다.

오브젝트를 생성하면 실행시간 라이브러리는 오브젝트의 상태를 저장하기 위해 메모리를 확보한다.
오브젝트의 인스턴스 변수들이 오브젝트 메모리 이미지를 구성한다.

오브젝트를 가리키는 변수가 val 이라면 해당 변수에 새로운 오브젝트를 대입하는 것은 불가능하지만,
오브젝트의 var 필드값은 오브젝트 변수 종류에 상관 없이 변경할 수 있다.

탄탄한 오브젝트를 만들려면 오브젝트 생명주기 동안 오브젝트의 상태, 즉 인스턴스 변수를 항상 유효한 값으로 유지하는 것이 중요하다.
이를 위한 첫번 째 조치는 private 필드를 만들어서 외부에서 필드에 바로 접근하지 못하게 하는 것이다.
private 필드는 같은 클래스에 정의되어 있는 메서드에서만 접근할 수 있기 때문에
오브젝트 상태를 변경할 수 있는 모든 코드를 클래스 내부에 격리할 수 있다.

	class ChecksumAccumulator {
		private var sum = 0
	}

외부에서 sum 에 접근할 수 없게 되었다.

	val acc = new ChecksumAccumulator
	acc.sum = 5 // Won’t compile, because sum is private

자바에서는 공개용 멤버들을 public 이라 명시적으로 표시하지만
스칼라에서는 접근 제한이 없는 멤버들은 자동으로 public 이 되니 따로 적을 필요가 없다.

sum 을 수정하는 코드를 클래스 내부에 정의해 보았다.

	class ChecksumAccumulator {
		private var sum = 0

		def add(b: Byte): Unit = {
			sum += b
		}

		def checksum(): Int = {
			return ~(sum & 0xFF) + 1
		}
	}

스칼라 메서드의 인자는 var 가 아니라 val 이다.
인자값은 변경할 수 없다.

add 와 checksum 메서드를 좀더 간단하게 만들어 보자.

먼저 checksum 메서드 마지막 return 은 생략할 수 있다.
메서드 마지막 수식의 값은 자동으로 리턴된다.
명시적인, 특히나 여러번 등장하는 return 사용은 피하길 권한다.
대신 각각의 메서드를 하나의 값을 도출하는 수식으로 생각하라.
이 철학을 따르면 큰 메서드를 작은 메서드들로 인수분해하게 된다.

메서드가 하나의 수식만 갖는다면 { } 를 생략할 수 있다.
수식이 간단할 경우 def 와 같은 줄에 적는 것도 괜찮다.

	class ChecksumAccumulator {
		private var sum = 0
		def add(b: Byte): Unit = sum += b
		def checksum(): Int = ~(sum & 0xFF) + 1
	}

add 와 같이 리턴 타입이 Unit 인 메서드는 부수효과(side-effects)를 유발한다.
부수효과는 메서드 외부의 상태를 변화시키거나 I/O 를 발생시킨다.
add 의 경우 sum 에 새로운 값을 대입하고 있다.

Unit 을 리턴하는 메서드는 리턴 타입 Unit 과 = 를 생략할 수 있다.
단 이렇게 되면 { } 는 생략할 수 없다.
이런 형태의 메서드는 프로시저 처럼 보인다.
(파스칼 등에서 프로시저라는 단어가 사용되는데 리턴 타입 없이 오로지 부수효과를 만들기 위한 루틴을 지칭한다.)

	class ChecksumAccumulator {
		private var sum = 0
		def add(b: Byte) { sum += b }
		def checksum(): Int = ~(sum & 0xFF) + 1
	}


### 참고

생성자 설명은 어디 갔는가!
스칼라의 오브젝트 초기화는 자바와 많이 다르다.
다행히 훨씬 간편하다.
이에 대한 설명은 다음 다음 쳅터 Functional Object 에서 하기로 한다.

스칼라에는 static 멤버가 없다.
모든 것은 오브젝트를 통해서 한다.
자바에서 static 멤버로 하던 일은 클래스 보조 오브젝트로 한다.
