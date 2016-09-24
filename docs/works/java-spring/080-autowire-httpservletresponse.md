---
title: Autowire HttpServletResponse
---

2011-09-08

`HttpServletRequest` 는 쉽게 Autowire 해서 사용할 수 있지만 `HttpServletResponse` 는 Autowire 할 수가 없습니다.
1 년 전엔 귀찮아서 안 했었는데 Code-based Configuration 방식으로 관련 클래스와 설정을 한 곳에 몰아서
`HttpServletResponse` 에 Autowire 를 걸어봤습니다.

    @Configuration
    @EnableTransactionManagement
    @EnableWebMvc
    public class SpringSetter extends WebMvcConfigurerAdapter {
      @Bean
      public HttpServletResponseInScopeInterceptor httpServletResponseInScopeInterceptor() {
        return new HttpServletResponseInScopeInterceptor();
      }
    
      @Override
      public void configureInterceptors(InterceptorConfigurer configurer) {
        configurer.addInterceptor(httpServletResponseInScopeInterceptor());
        configurer.addInterceptor(...);
        configurer.addInterceptor(...);
        configurer.addInterceptor(...);
      }
    
      @Bean
      public HttpServletResponseFactoryBean httpServletResponseFactoryBean() {
        return new HttpServletResponseFactoryBean();
      }
    
      private static class HttpServletResponseInScopeInterceptor extends HandlerInterceptorAdapter {
    
        private ThreadLocal<HttpServletResponse> localResponse = new ThreadLocal<HttpServletResponse>();
    
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
          localResponse.set(response);
          return true;
        }
    
        @Override
        public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
          localResponse.remove();
        }
    
        private HttpServletResponse getHttpServletResponse() {
          return localResponse.get();
        }
    
        public void destroy() {
        }
      }
    
      private static class HttpServletResponseFactoryBean implements FactoryBean<HttpServletResponse> {
    
        @Autowired private HttpServletResponseInScopeInterceptor httpServletResponseInScopeInterceptor;
    
        public HttpServletResponse getObject() throws Exception {
          return httpServletResponseInScopeInterceptor.getHttpServletResponse();
        }
    
        public Class getObjectType() {
          return HttpServletResponse.class;
        }
    
        public boolean isSingleton() {
          return false;
        }
      }
    }

사용예

    @Component
    @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class Cookies {
    
      @Autowired private RequestContext requestContext;
      @Autowired private HttpServletRequest req;
      @Autowired private HttpServletResponse res;
    
    ...
  
    }


{:class="go-to-index"}
[Java Spring](index)
