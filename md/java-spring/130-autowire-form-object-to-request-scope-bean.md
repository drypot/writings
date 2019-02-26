---
title: Autowire Form Object to Request Scope Bean
---

2010-10-28

아래와 같은 핸들러 메서드가 있을 때,

    @RequestMapping(value = "/post", method = RequestMethod.GET)
      public String list(PostParams postParams) {
    }

`postParams` 오브젝트를 저 멀리 떨어진 request scope 에 자동 와이어링 할 수 있을까요?

    @Component
    @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class PostRing {
      @Autowired
      private PostParams postParams;
    }

결론은 안 됩니다. =,=

스프링 코드 돌아다니며 쭉 살펴봤는데 Model 오브젝트를 request attribute 에 넘기는 시점이 view 렌더링하기 바로 전이라
중간에 Model 을 스프링 밖에서 가져다 쓸 수가 없더군요.
하는 수 없이 수작업으로 넘겨야.


{:class="go-to-index"}
[Java Spring](index)
