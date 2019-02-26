---
title: Session Scope Bean
---

2010-10-15

Setvlet API 로 HTTP 세션을 구현하면
`Request` 에서 `getSession` 으로 `Session` 오브젝트 받아서 맵 타입으로 데이터를 넣고 뺍니다.

스프링에서는 `Session` 오브젝트를 받거나 맵 타입으로 관리하지 않고
세션 처리할 클래스 자체를 Autowire 해서 사용합니다.

말로하려니 이상한데 일단 걍 코드를 보면.

    @Component
    @Scope(value = "session", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class Seed {
      
      private int count;
      
      public int getCount() {
        return count++;
      }
      
      public void setCount(int count) {
        this.count = count;
      }
      
    }

무엇인가 세션처리하고 싶은 오브젝트 예입니다.
위와 같이 `@Scope` 어노테이션을 붙여주면 끝입니다.

사용할 때는 `Seed` 오브젝트를 와이어링해서 쓰면됩니다.

    @Controller
    public class PostController {
    
      @Autowired
      private Seed seed;
      
      @RequestMapping(value = "/post", method = RequestMethod.GET)
      @ResponseBody
      public String create() {
        return "Hello " + seed.getCount();
      }
    }


일반적인 스프링오브젝트와 세션 스코프 스프링오브젝트가 다른 점은
세션 스코프의 경우 우리가 만든 `Seed` 클래스의 오브젝트가 바로 들어와 붙는 것이 아니고
(그렇게 되면 세션 오브젝트가 아니고 글로벌 오브젝트 처럼 활동하겠지요)
세션별로 Seed 오브젝트를 만들어 관리하는 프락시를 대신 인젝션 해줍니다.

어쨌든 결론은 `Session` 오브젝트 관리하기 위해 `getSession` 하고,
`map` 에 넣고 빼고 하는 노가다 작업은 필요 없어졌습니다.


{:class="go-to-index"}
[Java Spring](index)
