---
title: Scala Is High-Level
---

2011-07-08 00:17

### Scala Is High-Level / 스칼라는 추상적이다.

프로그래머들은 복잡성과 싸워 왔다.
너무 복잡한 코드는 몰락하는 많은 소프트웨어 프로젝트들의 원인이 되었다.
불행히도 중요한 소프트웨어들은 복잡한 요구사항들을 가진다.
이 복잡성은 피할 수는 없지만 세심하게 관리할 수는 있다.

스칼라는 인터페이스의 추상화 단계를 높임으로써 복잡성을 줄인다.
예를 들어 name 이란 이름의 String 이 대문자를 포함하고 있는지 확인한다고 하자.
자바에서는 다음과 같이 적을 것이다.

    // this is Java
    boolean nameHasUpperCase = false;
    for (int i = 0; i < name.length(); ++i) {
      if (Character.isUpperCase(name.charAt(i))) {
        nameHasUpperCase = true;
        break;
      }
    }

스칼라에서는 이것을 다음과 같이 적을 수 있다.

    val nameHasUpperCase = name.exists(x => x.isUpper)

이렇게 복잡한 절차적 구문을 라이브러리화 하는 컨트롤 추상화는
코드의 중복을 막고 여러분의 프로그램을 더 작고 깔끔하게 유지하는데 도움이 된다.

자바에서 스트링은 루프를 통해 개개의 문자를 다루는 존재고,
스칼라에서 스트링은 술어를 가지고 질문을 던질 수 있는 고급 존재가 된다.

술어 x => x.isUpper 는 스칼라 펑션 리터럴의 예다.
이것은 문자를 인자로 받아서 그것이 대문자인지 테스트하는 펑션을 나타낸다.
같은 기능의 술어를 아래와 같이 더 짧게 적을 수도 있다.

    val nameHasUpperCase = name.exists(_.isUpper)

(참고로, 스칼라에서 _ 은 서로 다른 곳에서 매우 다양한 의미로 사용된다.)

원칙적으로 이와 같은 컨트롤 추상화는 자바에서도 가능하다.
이를 구현하려면 인터페이스를 정의해야 하고, 이 인터페이스는 추상화할 기능을 묘사하는 메서드를 가질 것이다.
예를 들어 스트링에 질의를 던지고 싶다면 hasProperty 라는 메서드를 갖는 ChracterProperty 인터페이스를 만들어야 할 것이다.

    // this is Java
    interface ChracterProperty {
      boolean hasProperty(char ch);
    }

이 인터페이스로 exists 메서드를 만들 수 있다.
이 메서드는 문자열과 CharacterProperty 를 받아서 문자열에 이 속성을 만족하는 문자가 있다면 참을 돌려준다.
exists 메서드는 다음과 같이 부를 수 있다.

    // this is Java
    exists(name, new CharacterProperty() {
      public boolean hasProperty(char ch) {
        return Character.isUpperCase(ch);
      }
    });

하지만 이 모든 것은 너무 과하다.
사실 너무 과해서 대부분의 자바 개발자들은 절때 이렇게 하지 않을 것이다.
그냥 루프를 쓰고 만다.

반대로 스칼라의 펑션 리터럴은 진짜로 가볍다.
그래서 자주 사용된다.
스칼라에 익숙해 질 수록 컨트롤 추상화를 코드에 적용할 가능성을 더 많이 발견하게 될 것이다.

### 참고

펑션 리터럴을 언어학에서는 일반적으로 람다라고 한다.
가끔 람다 개념을 클로져라고 설명하는 책들이 있는데 클로저는 조금 다른 현상을 설명하는 단어다.
클로저 현상에 람다 구문이 개입하지만 다른 의미이니 혼동하지 않으셨으면.

컨트롤 추상화는 굉장히 중요한 개념이다.
절차적 언어에서는 중복 코드를 덩어리 채로만 뽑아 놓을 수 있지만,
펑셔널 언어는 이 덩어리의 일부 패턴만 뽑아서 추상화할 수 있다.
경우마다 달라지는 부분은  람다를 만들어 인자로 던지게 된다.

람다는 깔끔하고 쓰기 쉽다.
하지만 루프 단계마다 펑션 콜이 발생해서 루프에 비해 보통 두세 배 느리다.


{:class="go-to-index"}
[Programming in Scala](index)
