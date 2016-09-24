---
title: Spring 소개
---

2010-08-17

### Unit Testing 과 Dependency Injection

스프링을 이해하려면 Unit Testing 을 사용하는 개발자들이 있다는 것을 먼저 알 필요가 있다.
Unit Testing 은 Test Case 모듈을 사용해서 메인 모듈의 동작을 점검하는 개발전략이다.
그런데, Unit Testing 를 하다보면 자주 발생하는 난처한 상황이 있다.
예로 TwitterClient 를 만들어 테스트하려고 하면 테스트를 돌릴 때마다 실제 트위터 서버에 접속해야 한다.

    class TwitterClient {
      private TwitterConnection tc;
    
      public TwitterClient() {
        tc = new TwitterConnection("http://twiiter.com/...");
      }
    }

이 문제를 어떻게 해결해야 할까? 간단하다. 
메서드 내부에서 `TwitterConnection` 오브젝트를 만드는 대신
외부에서 테스트용 `TwitterConnection` 오브젝트를 만들어 전달하면 된다.

    class TwitterClient {
      private TwitterConnection tc;
    
      public TwitterClient(TwitterConnection tc) {
        this.tc = tc;
      }
    }

별 차이 없는 것 같지만 사용할 오브젝트를 외부에서 제공 받는 이런 방식은 프로그램의 유연성을 대폭 증가시켜준다.
그리고, 누가 이 방식에 Dependency Injection 이라고 이름을 붙였다. 

### Spring

Unit Testing 을 주도적으로 사용하는 개발을 진행 방식을 Test Driven Development 라고 한다.
위의 예에서 처럼 TDD 를 하다보면 DI 가 많이 발생하게 되는데 이 대규모 DI 의 자동화를 위해 만든 프레임웍이 스프링이다.
이후 DB Tempalte, MVC, Transaction, AOP 등 많은 부속 모듈들이 하나 둘 추가되었다.
스프링 우산 아래에 제공되는 이런 최신 기능들은 일선 개발자들이 처한 현실적인 문제들에 권장할만한 해법들을 제공하고 있다.

스프링 학습을 시작하게 되면 일단 DI, AOP 등 스프링 환경에서 전반적으로 사용되는 기본 개념을 익히고,
이후 필요에 따라 스프링 서브 모듈들의 사용법을 하나하나 알아나가면 된다.
TDD 를 하지 않으면 DI 와 처음엔 스프링의 필요성이 잘 느껴지지 않을 수도 있다.
하지만 스프링을 쓰기 시작하면 DI 에 익숙해지는 것을 시작으로 스프링이 제공하는 좋은 개발전략들을 자연스럽게 접할 수 있다.

### 해야하나요?

"스프링 해야하나요?"  
"네. 당신이 회사에서 자바로 웹 사이트 만드는 일을 하고 있다면 보셔야합니다."

"다른 괜찮은 대안은 없나요?"  
"없습니다."


{:class="go-to-index"}
[Java Spring](index)
