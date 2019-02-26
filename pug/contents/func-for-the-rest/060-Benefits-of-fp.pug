---
title: Benefits of FP / 잇점
---

2011-09-20

원문: <http://www.defmacro.org/ramblings/fp.html>{:target="_blank"}

You're probably thinking that there's no way I can rationalize the monstrosity of a function above. When I was learning functional programming I was thinking that too. I was wrong. There are very good arguments for using this style. Some of them are subjective. For example, people claim that functional programs are easier to understand. I will leave out these arguments because every kid on the block knows that ease of understanding is in the eye of the beholder. Fortunately for me, there are plenty of objective arguments.

이전 글에서 소개한 괴상한 펑션에 납득할 만한 설명을 붙이는 것이 불가능하리라 생각할 수도 있겠습니다. 저도 FP 를 배우면서 그렇게 생각했습니다. 하지만 틀렸었습니다. 변수를 쓰지 않는 이런 스타일에 대한 합리적인 해석들은 매우 많습니다. 주관적인 것들이 있기는 합니다. 예를들어 펑셔널 프로그램이 더 이해하기 쉽다는 사람들이 더러 있는데 이에 대한 논의는 제외하도록 하겠습니다. 이해가 쉬운지 아닌지는 보는 사람에 따라 다르기 때문입니다. 제가 이제부터 적으려는 것은 객관적인 평가들에 관한 것입니다.

### Unit Testing / 유닛 테스팅

Since every symbol in FP is final, no function can ever cause side effects. You can never modify things in place, nor can one function modify a value outside of its scope for another function to use (like a class member or a global variable). That means that the only effect of evaluating a function is its return value and the only thing that affects the return value of a function is its arguments.

FP 에서는 모든 심볼이 final 이기 때문에 어떤 펑션도 부수 효과 (side effect) 를 발생시킬 수 없습니다. 이미 사용중인 어떤 것도 변경할 수 없습니다. 클래스 멤버라든지, 전역 변수도 사용할 수 없습니다. 펑션 외부의 값을 변경해서 다른 펑션에서 사용하는 일은 불가능합니다. 다시 말해 펑션 호출과정에서 발생할 수 있는 유일한 효과는 펑션 리턴값이며 리턴값에 영향을 줄 수 있는 유일한 것은 펑션 인자뿐입니다.

This is a unit tester's wet dream. You can test every function in your program only worrying about its arguments. You don't have to worry about calling functions in the right order, or setting up external state properly. All you need to do is pass arguments that represent edge cases. If every function in your program passes unit tests you can be a lot more confident about quality of your software than if you were using an imperative language. In Java or C++ checking a return value of a function is not sufficient - it may modify external state that we would need to verify. Not so in a functional language.

유닛 테스터들이 꿈에 그리던 이상이군요. 프로그램의 모든 펑션을 테스트하는데 펑션 인자만 신경쓰면 된다는 말입니다. 펑션을 적절한 순서대로 호출해야 한다든지, 외부 상태를 적절히 세팅해야 한다든지 하는 고민거리가 없습니다. 한계 상황을 나타내는 인자를 전달하는 것이 전부입니다. 프로그램의 모든 펑션이 유닛 테스트를 통과한다면 절차적인 언어를 사용할 때보다 소프트웨어의 질에 더 큰 확신을 가질 수 있습니다. 자바나 C++ 에서는 펑션의 리턴값을 확인하는 것으로 충분하지 않습니다. 외부 상태를 변경할 수도 있어서, 이것까지 확인해야할 수도 있습니다.

### Debugging / 디버깅

If a functional program doesn't behave the way you expect it to, debugging it is a breeze. You will always be able to reproduce your problem because a bug in a functional program doesn't depend on unrelated code paths that were executed before it. In an imperative program a bug resurfaces only some of the time. Because functions depend on external state produced by side effects from other functions you may have to go through a series of steps in no way related to the bug. In a functional program this isn't the case - if a return value of a function is wrong, it is always wrong, regardless of what code you execute before running the function.

기대한 대로 동작하지 않는 펑셔널 프로그램은 간단하게 디버깅할 수 있습니다. 펑셔널 프로그램에서의 버그는 그 이전에 실행된 코드에 의존적이지 않기 때문에 문제를 쉽게 재현할 수 있습니다. 절차적인 프로그램에서는 오동작이 나타나기도 하고 안 나타나기도 합니다. 절차적 프로그램의 펑션은 다른 펑션의 부수 효과에 의존적이기 때문에 버그와 직접 관련이 없는 절차들을 거쳐야 할 수도 있습니다. 펑셔널 프로그램은 그렇지 않습니다. 펑션의 리턴값이 틀리다면, 그 리턴값은 항상 틀립니다. 그 펑션 이전에 어떤 코드를 실행했는가는 상관이 없습니다.

Once you reproduce the problem, getting to the bottom of it is trivial. It is almost pleasant. You break the execution of your program and examine the stack. Every argument in every function call in the stack is available for your inspection, just like in an imperative program. Except in an imperative program that's not enough because functions depend on member variables, global variables, and the state of other classes (which in turn depend on these very same things). A function in a functional program depends only on its arguments, and that information is right before your eyes! Furthermore, in an imperative program examining a return value of a function will not give you a good idea of whether the function behaves properly. You need to hunt down dozens of objects outside its scope to verify that it performed correct actions. In a functional program all you have to do is look at the return value!

문제를 재현할 수 있으면 원인을 파보는 것은 간단한 일입니다. 작업을 기분 좋게 마무리할 수 있습니다. 프로그램의 실행경로를 분석하고 스택을 조사합니다. 펑션 호출에 사용된 모든 인자들이 스택에 있어서 그대로 들여다 볼 수 있습니다. 이것은 절차적 프로그램에서도 가능한 일이지만 절차적 프로그램에서는 이것이 전부가 아닙니다. 절차적 프로그램의 펑션은 멤버 변수, 전역 변수, 다른 클래스들의 상태에 의존적입니다. 이 클래스들은 또 다른 것들에 의존적입니다. 반해서, 펑셔널 프로그램의 펑션은 오로지 자신의 인자에만 의존적입니다. 항상 바로 눈앞에 있습니다. 더해서, 절차적 프로그램에서는 펑션의 리턴값을 조사한다고 펑션이 적절히 동작했는지 총체적으로 확신할 수 없습니다. 펑션의 외부에 있는 수 많은 오브젝트들을 찾아서 펑션이 적절한 행동을 했는지 확인해야 합니다.

Walking through the stack you look at arguments passed to functions and their return values. The minute a return value doesn't make sense you step into the offending function and walk through it. You repeat this recursively until the process leads you to the source of the bug!

스택을 검토하면서 펑션에 넘겨진 인자나 리턴값을 볼 수 있습니다. 리턴값이 잘못되었다면 문제가 된 펑션안으로 들어가서 펑션을 검토합니다. 버그의 원인에 닿을 때까지 이 과정을 재귀적으로 반복하면 끝입니다.

### Concurrency / 병렬처리

A functional program is ready for concurrency without any further modifications. You never have to worry about deadlocks and race conditions because you don't need to use locks! No piece of data in a functional program is modified twice by the same thread, let alone by two different threads. That means you can easily add threads without ever giving conventional problems that plague concurrency applications a second thought!

펑셔널 프로그램은 별다른 수정 없이 병렬처리에 바로 사용가능합니다. 락을 사용할 필요가 없기 때문에 데드락이나 경쟁 상태에 관해 걱정할 필요가 없습니다. 펑셔널 프로그램에서는 같은 스레드에 의해서 건 서로 다른 스레드에 의해서 건, 어떤 데이터도 두 번 수정되지 않습니다. 이 말은 스레드를 추가하는데 병렬처리 프로그램을 작성을 귀찮게 만드는 관례적인 문제들을 고민할 필요가 없다는 말입니다.

If this is the case, why doesn't anybody use functional programs for highly concurrent applications? Well, it turns out that they do. Ericsson designed a functional language called Erlang for use in its highly tolerant and scalable telecommunication switches. Many others recognized the benefits provided by Erlang and started using it. We're talking about telecommunication and traffic control systems that are far more scalable and reliable than typical systems designed on Wall Street. Actually, Erlang systems are not scalable and reliable. Java systems are. Erlang systems are simply rock solid.

만약 이것이 사실이라면 고강도 병렬처리 어플리케이션에 왜 펑셔널 프로그램을 쓰지 않을까요? 왜요, 씁니다. 에릭슨은 매우 안정적이고 확장성있는 전화 교환기를 만들기 위해 얼랭이라는 펑셔널 언어를 디자인했습니다. 많은 사람들이 얼랭의 잇점을 깨닫고 사용을 시작했습니다. 월스트리트에서 디자인된 전형적인 시스템보다 훨씬 확장성 있고 안정성있는 전화나 교통 통제 시스템에 대해서 말하고 있는 겁니다. 사실 얼랭 시스템에 확장성 있다거나 안정성 있다는 말은 어울리지 않습니다. 자바에게나 어울리지요. 얼랭 시스템은 그냥 끝장이다라는 말이 어울립니다.

The concurrency story doesn't stop here. If your application is inherently single threaded the compiler can still optimize functional programs to run on multiple CPUs. Take a look at the following code fragment:

병렬처리 이야기는 여기서 끝나지 않습니다. 어플리케이션이 한 스레드에 의해 운영되도록 만들졌다 해도 컴파일러는 펑셔널 프로그램을 최적화해서 다중 CPU 에서 실행되도록 할 수 있습니다. 

    String s1 = somewhatLongOperation1();
    String s2 = somewhatLongOperation2();
    String s3 = concatenate(s1, s2);

In a functional language the compiler could analyze the code, classify the functions that create strings s1 and s2 as potentially time consuming operations, and run them concurrently. This is impossible to do in an imperative language because each function may modify state outside of its scope and the function following it may depend on it. In functional languages automatic analysis of functions and finding good candidates for concurrent execution is as trivial as automatic inlining! In this sense functional style programs are "future proof" (as much as I hate buzzwords, I'll indulge this time). Hardware manufacturers can no longer make CPUs run any faster. Instead they increase the number of cores and attribute quadruple speed increases to concurrency. Of course they conveniently forget to mention that we get our money's worth only on software that deals with parallelizable problems. This is a very small fraction of imperative software but 100% of functional software because functional programs are all parallelizable out of the box.

펑셔널 언어에서는 이런 것이 가능합니다. 컴파일러가 코드를 분석합니다. 스트링 s1 과 s2 생성하는 펑션이 시간 소모성 코드일 가능성이 있다고 판단합니다. 각 펑션을 병렬로 실행합니다. 절차적인 언어에서는 각 펑션이 외부 상태를 변경할 수 있고 이어지는 펑션들이 그 결과에 의존적일 수 있기 때문에 이런 옵티마이징은 불가능합니다. 펑셔널 언어에선 펑션을 자동 분석하고 적절한 병렬처리의 대상을 찾는 것이 자동 인라이닝 만큼 손쉬운 일입니다. 이런 맥락에서 펑셔널 스타일 프로그램은 미래에 폐물이 될 가능성이 적습니다. 하드웨어 생산자들은 더 이상 CPU 를 빠르게 만들 수 없습니다. 대신에 코어의 갯수를 늘려 병렬처리를 함으로써 속도를 4 배로 올리고 있습니다. 물론 병렬처리 문제를 극복한 소프트웨어만이 돈 값어치를 한다는 설명을 너무도 쉽게 빼먹습니다. 병렬처리 문제를 해결하고 출시되는 절차적인 프로그램의 비율은 매우 낮습니다. 반해서, 펑셔널 소프트웨어는 태생적으로 병렬처리성을 내제하기 때문에 모든 펑셔널 소프트웨어는 다중 CPU 에 대응합니다.

### Hot Code Deployment / 무중단 코드 배포

In the old days of Windows in order to install updates it was necessary to restart the computer. Many times. After installing a newer version of a media players. With Windows XP the situation has improved significantly, yet it still isn't ideal (I ran Windows Update at work today and now an annoying system tray icon won't go away until I restart). Unix systems have had a better model for a while. In order to install an update you only need to stop relevant components, not the whole OS. While it is a better situation, for a large class of server applications it still isn't acceptable. Telecommunication systems need to be up 100% of the time because if dialing emergency is not available due to upgrades, lives may be lost. There is no reason Wall Street firms need to bring down their systems to install software updates over the weekend.

예전 윈도우 환경에서 업데이트를 설치하려면 컴퓨터를 재부팅해야 했습니다. 여러번 재부팅해야 했습니다. 새버전의 미디어 플래이어를 설치한 후에도 재부팅해야 했습니다. XP 에 와서 상황이 나아졌지만 이상적이지는 않았습니다. 일하는 와중에 윈도우 업데이트를 설치하면 재부팅하기 전까지 귀찮은 트레이 아이콘이 사라지지 않았습니다. 유닉스 시스템들은 보다 좋은 모델을 가지고 있었습니다. 업데이트를 설치하려면 전체 운영체제가 아니라  관련 콤포넌트들만 중지시키면 되었습니다. 이 방법이 더 낫긴하지만 대규모 서버 어플리케이션에서는 이 조차 용납되지 않습니다. 업그레이드 중 응급전화를 걸지 못한다면 누군가의 생명이 위험할 수 도 있기 때문에 전화 시스템은 100% 가동률을 보여야 합니다. 월스트리트 회사의 경우는 소프트웨어 업데이트를 설치한다고 주말까지 기다릴  이유도 없습니다.

An ideal situation is updating relevant parts of the code without stopping any part of the system at all. In an imperative world this isn't possible. Consider unloading a Java class at runtime and reloading a new definition. If we were to do that every instance of a class would become unusable because the state it holds would be lost. We would need to resort to writing tricky version control code. We'd need to serialize all running instances of the class, destroy them, create instances of the new class, try to load serialized data into them hoping the loading code properly migrates the data to work with the new instance. On top of that, every time we change something we'd have to write our migration code manually. And our migration code would have to take special care not to break relationships between objects. Nice in theory, but would never work well in practice.

이상적인 상황은 시스템의 어떤 부분도 중지시키지 않고 코드의 적절한 부분을 업데이트하는 것입니다. 절차적인 세계에서 이러한 일은 불가능합니다. 실행중에 자바 클래스를 해제하고 새로운 코드를 읽어들이는 상황을 생각해 보십시오. 그렇게 했다간 클래스의 모든 인스턴스들이 저장하고 있던 상태를 잃게 되어 아예 사용하지 못하게 될 것입니다. 문제를 해결하기 위한 교묘한 버전 컨트롤 코드를 만들어야할 수도 있습니다. 아니면, 교체하려는 클래스의 모든 인스턴스를 시리얼라이즈하고, 오브젝트를 해제한 후, 새로운 클래스의 인스턴스를 만들어서 시리얼라이즈된 데이터를 읽어들여야 할 수도 있습니다. 로딩 코드는 새 인스턴스에 사용될 수 있도록 데이터를 적절히 변환해야 할 겁니다. 이게 끝이 아니고, 무엇인가 수정할 때마다 변환 코드를 수작업으로 만들어야 합니다. 변환 중에 오브젝트간의 관계가 깨지지 않도록 변환 코드에 세심한 주의도 기울여야합니다. 이론은 좋습니다만, 결국 현실성은 없죠. 

In a functional program all state is stored on the stack in the arguments passed to functions. This makes hot deployment significantly easier! In fact, all we'd really have to do is run a diff between the code in production and the new version, and deploy the new code. The rest could be done by language tools automatically! If you think this is science fiction, think again. Erlang engineers have been upgrading live systems without stopping them for years.

펑셔널 프로그램에서는 모든 상태가 펑션에 전달된 인자로 스텍에 저장됩니다. 그래서 운행중에도 쉽게 코드를 변경할 수 있습니다. 실행중인 버전과 새 버전간에 diff 를 돌려서 새 코드를 적용하는 것이 전부입니다. 나머지는 언어 도구로 자동 처리됩니다. 이것이 공상과학 소설이라고 생각하신다면 다시 생각해 보세요. 얼랭 엔지니어들은 시스템 중단없이 수 년간 시스템을 업그레이드해 오고 있습니다. 

### Machine Assisted Proofs and Optimizations / 기계적 검증과 옵티마이징

An interesting property of functional languages is that they can be reasoned about mathematically. Since a functional language is simply an implementation of a formal system, all mathematical operations that could be done on paper still apply to the programs written in that language. The compiler could, for example, convert pieces of code into equivalent but more efficient pieces with a mathematical proof that two pieces of code are equivalent7. Relational databases have been performing these optimizations for years. There is no reason the same techniques can't apply to regular software.

펑셔널 언어가 인상적인 점은 수학적으로 추론할 수 있다는 것입니다. 펑셔널 언어는 포멀 시스템의 직접적인 구현이기 때문에  논문에 사용할 수 있는 모든 수학적 전개는 펑셔널 언어로 작성된 프로그램에 그대로 적용할 수 있습니다. 예를 들어, 컴파일러는 코드의 일부분을 수학적 검증을 통해 등가이면서 보다 효율적인 코드로 변환할 수 있습니다. 관계형 디비는 이 작업을 수 년간 해오고 있습니다. 똑같은 기술이 일반 소프트웨어에 적용되지 못할 이유는 없습니다. 

Additionally, you can use these techniques to prove that parts of your program are correct. It is even possible to create tools that analyze code and generate edge cases for unit tests automatically! This functionality is invaluable for rock solid systems. If you are designing pace makers and air traffic control systems such tools are almost always a requirement. If you are writing an application outside of truly mission critical industries, these tools can give you a tremendous edge over your competitors.

더 나아가 이런 기술들을 프로그램의 부분 부분이 정확한지 검증하는데 사용할 수도 있습니다. 코드를 분석해서 유닛 테스팅을 위한 극한 상황을 자동 생성하는 도구를 만드는 것도 가능합니다. 이 기능은 완전무결한 시스템을 만드는데 너무나도 큰 역할을 합니다. 맥박 조정기나 항공 관제 시스템을 디자인한다면 이런 도구들은 거의 언제나 필수적입니다. 실패가 용납되지 않는 어플리케이션을 만들고 있는 것이 아니라도 이런 도구들을 가지고 있다면 경쟁자들보다 큰 우위를 갖게 될 것입니다.


{:class="go-to-index"}
[Functional Programming For The Rest Of Us](index)
