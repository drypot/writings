---
title: ResponstBody Encoding
---

2010-10-04

컨트롤러 메서드에 `@ResponseBody` 를 붙여서
리턴 String 을 HttpResponse 로 자동으로 만들도록 할 수가 있었습니다.
그런데 이때 ContextType (인코딩) 은 어떻게 지정할까요?

할 수가 없습니다. =,=

<http://stackoverflow.com/questions/3616359/who-sets-response-content-type-in-spring-mvc-responsebody>

할 수가 없는 것은 아니나 스프링이 이부분에 있어서 기능이 미흡하므로
위에 글 처럼 `BeanPostProcessor` 붙여서 땜빵해야하는데 원하시면 저렇게 하셔도 되고,
아니면 아래 처럼 메서드 마다 `HttpServletResponse` 받아서 직접 넣으셔도 되겠습니다.

    @RequestMapping(value = "/account/get", method = RequestMethod.GET)
    //@ResponseBody
    public void /*String*/ get(String udid, HttpServletResponse res) throws IOException {
      try {
        Account account = accountService.getByUdid(udid);
        StringBuilder buf = new StringBuilder(512);
        buf.append( ... );
        res.setContentType("text/plain; charset=\"UTF-8");
        res.getWriter().append(buf);
      } catch(NoResultException e) {
        res.getWriter().println("-1");
      }
    }


{:class="go-to-index"}
[Java Spring](index)
