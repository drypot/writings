---
title: Autowire HttpServletRequest
---

2010-10-19

Session 스콥 빈 선언을 어떻게 하는지는 아래 글에서 적었었습니다.
아래 예처럼 클래스를 정의하고 @Autowired 하면 되었었습니다.

    @Component
    @Scope(value = "session", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class AppSession {
      @Autowired
      private HttpServletRequest request; 
    }


Session 스콥 빈에서 HttpServletRequest 를 쓰려면 어떻게 해야할지 고민이 되었는데 (문서에도 안 보이고 =,=)
위에 처럼 HttpServletRequest 타입으로 @Autowired 하면 되게 되어있습니다.
일반 Request 스콥 빈에서 HttpServletRequest 가 필요할 때도 위에 처럼 인젝트하시면 됩니다.
HttpServletRequest 는 당연 리퀘스트 스콥입니다.

오늘 반나절 삽질하게 만든 문제는 HttpServletRequest  는 되는데 HttpServletResponse 는 안 된다는 겁니다.
소스코드 뒤벼보니 Request 만 빈 펙토리에 세팅하고 있습니다. =,=

해법이 있긴 한데 아래 처럼 필터를 만들어서 수작업으로 펙토리를 넣어줘야 하는군요.
지저분해서 안 쓰고 말겠다능. =,=

<http://forum.springsource.org/showthread.php?t=88804>

그래도 Request 만이라도 인젝트 해서 쓸 수 있으니 지저분하게 Request 인자패스 하는 일은 줄어들 것 같습니다.


{:class="go-to-index"}
[Java Spring](index)
