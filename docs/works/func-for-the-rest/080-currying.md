---
title: Currying
---

2011-09-22

원문: <http://www.defmacro.org/ramblings/fp.html>{:target="_blank"}

Most people I've met have read the Design Patterns book by the Gang of Four. Any self respecting programmer will tell you that the book is language agnostic and the patterns apply to software engineering in general, regardless of which language you use. This is a noble claim. Unfortunately it is far removed from the truth.

제가 만나본 대부분의 사람들이 GoF 의 Design Patterns 를 읽었습니다. 자신감에 차있는 프로그래머라면 이 책의 내용이 특정 언어에 국한되지 않는다는 것과 패턴이란 어떤 언어를 사용하는지와 상관 없이 소프트웨어 엔지니어링 전반에 적용된다는 것을 강조할 겁니다. 이건 참 때 묻지 않은 주장입니다. 안타깝게도 진실은 전혀 다릅니다.

Functional languages are extremely expressive. In a functional language one does not need design patterns because the language is likely so high level, you end up programming in concepts that eliminate design patterns all together. Once such pattern is an Adapter pattern (how is it different from Facade again? Sounds like somebody needed to fill more pages to satisfy their contract). It is eliminated once a language supports a technique called currying.

펑셔널 언어들은 극단적인 표현성을 가지고 있습니다. 펑셔널 언어에서는 디자인 패턴이 필요가 없습니다. 펑셔널 언어는 보통 꽤 고기능이라서 디자인 패턴들을 대체할 수 있는 다른 개념들을 사용해 프로그래밍하게 됩니다. 디자인 패턴에 어댑터 패턴이란 것이 있습니다. 언어에서 커링이란 기술을 사용할 수 있으면 어댑터 패턴을 사용하지 않아도 됩니다. (근데 어댑터 패턴과 파사드 패턴의 차이점에 대해서 누가 알려 주시겠습니까? 패턴 명세를 보충하기 위해서 누군가 몇 페이지 더 써야겠더군요.) 

Adapter pattern is best known when applied to the "default" abstraction unit in Java - a class. In functional languages the pattern is applied to functions. The pattern takes an interface and transforms it to another interface someone else expects. Here's an example of an adapter pattern:

어댑터 패턴은 인터페이스를 다른 누군가가 원하는 제 3 의 인터페이스로 변환하는 것입니다. 자바의 기본 추상화 단위인 클래스에 적용되는 경우가 가장 잘 알려져 있지만, 펑셔널 언어에서 패턴은 펑션에 적용된다고 할 수 있으므로, 여기서는 어댑터 패턴이 펑션에 쓰이는 예를 보겠습니다. 

    int pow(int i, int j);
    
    int square(int i)
    {
        return pow(i, 2);
    }

The code above adapts an interface of a function that raises an integer to an integer power to an interface of a function that squares an integer. In academic circles this trivial technique is called currying (after a logician Haskell Curry who performed mathematical acrobatics necessary to formalize it). Because in FP functions (as opposed to classes) are passed around as arguments, currying is used very often to adapt functions to an interface that someone else expects. Since the interface to functions is its arguments, currying is used to reduce the number of arguments (like in the example above).

위 코드는 두 정수를 이용해 거듭 제곱을 구하는 펑션 `pow` 의 인터페이스를 한 정수만 받아 제곱을 구하는 펑션 `square` 로 변환합니다. 펑션의 인터페이스는 인자이므로 FP 에서 인터페이스를 바꾼다는 말은 펑션의 인자를 바꾼다는 말입니다. 위의 예 처럼 특정 인자를 고정해 더 적은 수의 인자를 받는 새로운 포장 펑션을 만드는 작업을 Partial Function Application 이라고 부릅니다. 또 이걸 Currying 이라 부르기도 합니다.

(역자주: Currying 과 Partial Function Application 은 서로 다른 개념입니다. 엄밀히 이 둘은 전혀 다른 것인데 PFA 를 Currying 이라 불러버리는 경우가 무시할 수 없을 정도로 많습니다. 이 글에서 설명하는 것도 엄밀히는 PFA 입니다. 개발자가 범용언어를 사용할 때는 주로 PFA 를 만나므로 당장 문제는 없겠지만, 혹시나 추후 진짜 Currying 을 만나게 될 때는 당황하지 마시고 위키피디아를 찾아보십시오. 요컨데, 위 설명은 PFA 인데 이걸 Currying 이라 부르는 언젠가부터의 잘못된 문화가 있습니다.

Currying 은 논리학자였던 Haskell Curry 에서 따온 이름입니다. Haskell Curry 는 커링을 공식화하는데 필요한 수학적 곡예를 감행했던 사람입니다. Haskell 언어도 이분 이름에서 유래합니다.)

Functional languages come with this technique built in. You don't need to manually create a function that wraps the original, functional languages will do that for you. As usual, let's extend our language to support this technique.

펑셔널 언어에는 PFA 기능이 내장되어 있습니다. 원래 펑션을 감쌀 펑션을 수작업으로 만들지 않아도 됩니다. 이 가술을 지원하기위해 자바를 확장해 봅시다. 

    square = int pow(int i, 2);

This will automatically create a function square for us with one argument. It will call pow function with the second argument set to 2. This will get compiled to the following Java code:

인자 한 개를 받는 펑션 `square` 는 그냥 이렇게 만들 수 있습니다. `sqaure` 는 두 번 째 인자를 `2` 로 고정해서 펑션 `pow` 를 호출합니다. 이 코드는 다음 자바 코드로 컴파일됩니다.

    class square_function_t {
        int square(int i) {
            return pow(i, 2);
        }
    }
    
    square_function_t square = new square_function_t();

As you can see, we've simply created a wrapper for the original function. In FP currying is just that - a shortcut to quickly and easily create wrappers. You concentrate on your task, and the compiler writes the appropriate code for you! When do you use currying? This should be easy. Any time you'd like to use an adapter pattern (a wrapper).

보시다 시피 원래 펑션에 대한 포장이 만들어졌습니다. 이와같이 펑셔널 언어에서는 PFA 를 쉽게 할 수 있습니다. 컴파일러가 적당한 코드를 대신 생성하기 때문에 여러분은 원래 하려던 일에 집중할 수 있습니다. 언제  PFA 를 써야할까요? 쉽습니다. 포장이 필요할 때, 즉 어댑터 패턴이 필요할 때는 항상 쓸 수 있습니다.

(이번 번역은 개작된 부분이 많았습니다.)


{:class="go-to-index"}
[Functional Programming For The Rest Of Us](index)
