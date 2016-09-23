---
title: Higher Order Functions / 고차 펑션
---

2011-09-20

원문: <http://www.defmacro.org/ramblings/fp.html>{:target="_blank"}

I remember learning about the benefits I outlined above and thinking "that's all very nice but it's useless if I have to program in a crippled language where everything is final." This was a misconception. Making all variables final is crippled in a context of an imperative language like Java but it isn't in a context of functional languages. Functional languages offer a different kind of abstraction tools that make you forget you've ever liked modifying variables. One such tool is capability to work with higher order functions.

이전 글에 약술한 잇점들을 학습하면서 “멋지다. 하지만 모든 것이 final 인 불완전한 언어를 가지고 프로그래밍해야 한다면 이런 잇점들은 모두 무용하지 않을까”라고 생각했던 것이 떠오릅니다. 이건 오해였습니다. 모든 변수를 final 로 만드는 것이 자바와 같은 절차적 언어의 맥락에선 망한 것으로 보일지 모르겠지만 펑셔널 언어의 맥락에선 그렇지 않습니다. 펑셔널 언어는 다른 종류의 추상화 도구들을 제공합니다. 변수를 다루는 것에 익숙했다는 사실 조차 잊게 만듭니다. 그런 도구중의 하나가 고차 펑션입니다.

A function in such languages is different from a function in Java or C. It is a superset - it can do all the things a Java function can do, and more. We create a function in the same manner we do in C:

펑셔널 언어의 펑션은 C 나 Java 의 펑션과는 좀 다릅니다. 그 슈퍼셋입니다. Java 펑션이 할 수 있는 일을 모두 할 수 있고, 더해서 그 이상을 할 수 있습니다. 펑셔널 펑션은 C 에서 하던 방식으로 작성합니다. (펑션을 만들기 위해 클래스를 깔지 않는다는 말입니다.)

    int add(int i, int j) {
        return i + j;
    }

This means something different from equivalent C code. Let's extend our Java compiler to support this notation. When we type something like this our compiler will convert it to the following Java code (don't forget, everything is final):

보기는 비슷하지만 펑셔널 펑션이 내포하는 의미는 C 펑션과는 좀 다릅니다. 위 표현을 지원하도록 자바 컴파일러를 확장했다면, 컴파일러는 이 코드를 다음과 같이 변환했을 것입니다. (JVM 에서는 모든 펑션이 클래스의 멤버여야하므로 클래스로 감샀습니다. 펑셔널 펑션은 OOP 에서 오브젝트의 능력을 갖습니다. 그래서 static 메서드로 변환하는 대신 인스턴스를 하나 만들었습니다. add 메서드는 항상 인스턴스를 통해 접근합니다.)

    class add_function_t {
        int add(int i, int j) {
            return i + j;
        }
    }

    add_function_t add = new add_function_t();

The symbol add isn't really a function. It is a small class with one function as its member. We can now pass add around in our code as an argument to other functions. We can assign it to another symbol. We can create instances of add_function_t at runtime and they will be garbage collected when we no longer need them. This makes functions first class objects no different from integers or strings. Functions that operate on other functions (accept them as arguments) are called higher order functions. Don't let this term intimidate you, it's no different from Java classes that operate on each other (we can pass class instances to other classes). We can call them "higher order classes" but nobody cares to because there is no strong academic community behind Java.

`add` 심볼은 (위에 메서드 `add` 말고, 아래 생성한 변수 `add`) 진짜 펑션은 아닙니다. 펑션 하나를 멤버로 갖는 작은 오브젝트입니다. 이렇게 오브젝트화 해두면 add 를 다른 펑션의 인자로 던질 수 있습니다. 다른 심볼에 대입할 수 있습니다. 펑션을 정수나 스트링같은 핵심 오브젝트처럼 다룰 수도 있습니다. 

다른 펑션을 인자로 받을 수 있는 펑션을 고차 펑션이라고 합니다. 용어에 섬득해할 필요는 없습니다. 이건 다른 클래스를 사용하는 클래스 이상의 의미가 없습니다. 한다고 하면 이런 클래스를 고차 클래스라고 부를 수는 있겠지만 아무도 그러진 않습니다.

How, and when, do you use higher order functions? Well, I'm glad you asked. You write your program as a big monolithic blob of code without worrying about class hierarchies. When you see that a particular piece of code is repeated, you break it out into a function (fortunately they still teach this in schools). If you see that a piece of logic within your function needs to behave differently in different situations, you break it out into a higher order function. Confused? Here's a real life example from my work.

고차 펑션은 언제, 어떻게 써야할까요? 네, 물어봐 줘서 고맙습니다. FP 에서는 클래스 계승에 고민하지 않고 한 덩어리로된 커다란 코드 뭉치를 만듭니다. 특정 부분의 코드가 반복된다면 이걸 펑션으로 뽑아냅니다. 펑션 안에서 일부 로직이 다른 조건에서 다르게 작동해야한다면 이 부분을 고차 함수로 뽑아냅니다. 헷갈리시나요? 예제를 보지요. 

Suppose we have a piece of Java code that receives a message, transforms it in various ways, and forwards it to another server.

메시지를 받아서 다양한 방식으로 변환한 뒤 또 다른 서버로 전달하는 자바 코드를 가정해 보겠습니다.

    class MessageHandler {
        void handleMessage(Message msg) {
            // ...
            msg.setClientCode("ABCD_123");
            // ...
        
            sendMessage(msg);
        }
    
        // ...
    }

Now imagine that our system has changed and we now route messages to two servers instead of one. Everything is handled in exactly the same way except the client code - the second server wants it in a different format. How do we handle this situation? We could check where the message is headed and format the client code differently, like this:

이제 시스템이 변경되어서 한 대 대신 두 대의 서버에 메시지를 전달해야 한다고 가정해 보겠습니다. 다른 것들은 동일한 방식으로 처리하지만 두 번째 서버가 다른 형식의 클라이언트 코드를 요구합니다. 이런 경우는 어떻게 처리해야 할까요? 다음과 같이, 메시지가 어디로 보내지고 있는지 확인해서 클라이언트 코드를 다르게 포맷해 보겠습니다. 

    class MessageHandler {
        void handleMessage(Message msg) {
            // ...
            if(msg.getDestination().equals("server1") {
                        msg.setClientCode("ABCD_123");
            } else {
                msg.setClientCode("123_ABC");
            }
            // ...
            
            sendMessage(msg);
        }
        
        // ...
    }

This approach, however, isn't scalable. If more servers are added our function will grow linearly and we'll have a nightmare updating it. An object oriented approach is to make MessageHandler a base class and specialize the client code operation in derived classes:

하지만 이 방법은 확장성이 없습니다. 서버가 추가되면 펑션 크기는 계속 증가해서 나중엔 업데이트하는 것이 악몽일 겁니다. OO 접근법은 `MessageHandler` 기초 클래스를 만든 후 클라이언트 코드 처리을 상속한 클래스에서 개별적으로 하는 것입니다.

    abstract class MessageHandler {
        void handleMessage(Message msg) {
            // ...
            msg.setClientCode(getClientCode());
            // ...
            
            sendMessage(msg);
        }
        
        abstract String getClientCode();
        
        // ...
    }
    
    class MessageHandlerOne extends MessageHandler {
        String getClientCode() {
            return "ABCD_123";
        }
    }
    
    class MessageHandlerTwo extends MessageHandler {
        String getClientCode() {
            return "123_ABCD";
        }
    }

We can now instantiate an appropriate class for each server. Adding servers becomes much more maintainable. That's a lot of code for such a simple modification though. We have to create two new types just to support different client codes! Now let's do the same thing in our language that supports higher order functions:

이제 서버별로 적당한 클래스 인스턴스를 만들 수 있습니다. 서버를 추가하는 작업이 훨씬 수월해 졌습니다. 하지만 이런 간단한 작업을 위해서 코드가 너무 많이 필요하군요. 단지 클라이언트 코드 차이를 지원하기 위해 새로운 타입을 두 개나 만들어야 했습니다. 이제 같은 문제를 해결하는데 고차 펑션을 사용해 보겠습니다. 

    class MessageHandler {
        void handleMessage(Message msg, Function getClientCode) {
            // ...
            Message msg1 = msg.setClientCode(getClientCode());
            // ...
            
            sendMessage(msg1);
        }
        
        // ...
    }
    
    String getClientCodeOne() {
        return "ABCD_123";
    }
    
    String getClientCodeTwo() {
        return "123_ABCD";
    }

    MessageHandler handler = new MessageHandler();
    handler.handleMessage(someMsg, getClientCodeOne);

We've created no new types and no class hierarchy. We simply pass appropriate functions as a parameter. We've achieved the same thing as the object oriented counterpart with a number of advantages. We don't restrict ourselves to class hierarchies: we can pass new functions at runtime and change them at any time with a much higher degree of granularity with less code. Effectively the compiler has written object oriented "glue" code for us! In addition we get all the other benefits of FP. Of course the abstractions provided by functional languages don't stop here. Higher order functions are just the beginning.

새로운 타입이나 클래스 족보를 만들지 않았습니다. 간단히 적절한 펑션을 인자로 전달할 뿐입니다. OOP 로 만든 것과 동일한 결과물을 얻었을 뿐만 아니라 몇 가지 장점도 얻었습니다. 클래스 체계의 제한을 받지 않습니다. 실행중에 새로운 펑션을 전달할 수 있습니다.  아무때나 더 작은 단위를 더 적은 코드를 가지고 변경할 수 있습니다. 결과적으로는 컴파일러가 우리를 대신해서 OO 스타일 연결 코드를 만들어 준 것입니다. 거기에 추가해서 FP 의 다른 모든 장점들을 얻었습니다. 물론 펑셔널 언어가 제공하는 추상화는 여기서 끝나지 않습니다. 고차 펑션은 단지 시작일 뿐입니다.


{:class="go-to-index"}
[Functional Programming For The Rest Of Us](index)
