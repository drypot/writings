---
title: Autowire HttpServletResponse 2
---

2010-10-19

전에 HttpServletResponse 를 인젝션하는 코드를 블로그에 올렸었는데 Session Scope Bean 에도 인젝션할 수 있도록 수정하였습니다. 과정에서 코드도 간단해졌습니다.

    @Configuration
    @EnableWebMvc
    public class SpringMVCSetter extends WebMvcConfigurerAdapter {
    
      @Override
      public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(httpServletResponseInScopeInterceptor());
      }
    
      private ThreadLocal<HttpServletResponse> localResponse = new ThreadLocal<HttpServletResponse>();
    
      @Bean
      public HandlerInterceptorAdapter httpServletResponseInScopeInterceptor() {
        return new HandlerInterceptorAdapter() {
          @Override
          public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            localResponse.set(response);
            return true;
          }
    
          @Override
          public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
            localResponse.remove();
          }
        };
      }
    
      @Bean
      @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
      public HttpServletResponse httpServletResponse() throws Exception {
        return localResponse.get();
      }
    
    }


{:class="go-to-index"}
[Java Spring](index)
