---
title: Spring MVC Security
---

2010-11-10

스프링 서브 프로젝트로 Spring Security 가 있습니다.
오래된 프로젝트이고, 웹 관련 보안에 관한 총제적인 솔루션입니다.
확장성도 좋고 스프링이 그렇듯이 다양한 인증 서비스에 대한 추상화도 잘 되어 있습니다.
문제는 만든지 오래되서 좀 노후하고 스프링 최신 기술들을 사용하지 않아서 쓰기가 편하질 않습니다.

현재는 Spring 3 의 MVC 기능을 사용해서 웹 개발을 하게 되는데
Spring Security 는 MVC 파이프라인을 타지 않고 Servlet Filter 파이프를 타기 때문에
어울리게 사용하기도 쉽지 않습니다.
그래서 Spring Security 를 쓰지 않고,
Spring MVC 에서 MVC Interceptor 가지고 페이지를 보호하는 간단한 예제를 만들었습니다.
컨트롤러 핸들러 메서드마다 로그인 확인하는 코드를 콜해도 되지만
비즈니스 로직과 기능 서비스 로직을 분리한다는 스프링 철학에 맞게 따로 떼었습니다.

    <mvc:interceptors>
      <bean name="securityInterceptor" class="com.drypot.sleek.interceptor.SecurityInterceptor"/>
    </mvc:interceptors>

스프링 설정파일에서 인터셉터 빈을 꼽아줍니다.
인터셉터 빈은 모든 MVC 콜에 대해 호출됩니다.
아마 하이버네이트 세션 관리를 위해서
`openSessionInViewInterceptor` 같은 인터셉터를 이미 쓰고 계실겁니다.
그 아래쯤 넣어 주시면 되겠습니다.

`mvc:interceptor` (끝에 s 없음) 태그로 특정 패스에 대해서만 실행되도록 설정할 수 있는데
이 작업은 시큐리티 인터셉터 내부에서 해도 되니 스프링 설정에서는 뺐습니다.

    public class SecurityInterceptor extends HandlerInterceptorAdapter {
      @Autowired private App app;
      @Autowired private AuthSession authSession;
      @Autowired private AuthService authService;
    
      @Override
      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String uri = request.getRequestURI();
        if (userRoleNeeded(uri)) {
          if (authSession.getRole().isUser()) {
            return true;
          }
          response.sendRedirect("/");
          return false;
        }
        return true;
      }
    
      private boolean userRoleNeeded(String uri) {
        return
          uri.startsWith("/post") ||
          uri.startsWith("/search") ;
      }
    }

인터셉터 구현 예입니다.
너무 간단해서 설명하기가 머슥하군요. =o=


{:class="go-to-index"}
[Java Spring](index)
