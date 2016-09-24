---
title: Bean Initialization with ServletContext
---

2010-11-30

일반적으로 스프링 빈에서 `ServletContext` 가 필요할 때는
`ServletContextAware` 인터페이스를 구현해 인젝션 받으면 됩니다.

하지만 `@PostConstruct` 어노테이션을 붙인 초기화 메서드에서 `ServletContext` 가 필요할 경우
`ServletContextAware` 의 `setServletContext` 메서드보다 `@PostConstruct` 메서드가 먼저 호출되기 때문에
`ServletContextAware` 를 사용할 수 없습니다.

이런 경우는 `ApplicationContextAware` 를 구현해서 `WebApplicationContext` 를 받은후 `ServletContext` 를 구합니다.

    @Component
    public class App implements ApplicationContextAware {
      private ServletContext servletContext;
    
      @Override
      public void setApplicationContext(ApplicationContext applicationContext) {
        servletContext = ((WebApplicationContext)applicationContext).getServletContext();
      }
    }


{:class="go-to-index"}
[Java Spring](index)
