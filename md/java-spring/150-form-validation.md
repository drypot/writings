---
title: Form Validation
---

2010-11-01

폼에서 데이터 입력 받고, 오류가 있는지 검사해서 오류가 있으면 오류내용을 출력해야 합니다.
Spring MVC 에서 오류 검사하는 초간단 시나리오입니다.

    @RequestMapping(value = "/post/new", method = RequestMethod.POST)
    public String newPost(PostForm postForm, Errors errors) {
      if (newPostHasErrors(errors, postForm)) return "post/new";
      ...
    }
    
    private boolean newPostHasErrors(Errors errors, PostForm postForm) {
      Post post = postForm.getPost();
      if ( ...  some error condition ... ) {
        errors.rejectValue("post.userName", "userName.notFilled");
      }
      return errors.hasErrors();
    }

컨트롤러와 에러 처리하는 간단 루틴 예입니다.
컨트롤러 메서드에 폼 오브젝트외에 Errors 타입 인자를 적어 놓으면 스프링에서 Errors 오브젝트를 자동으로 꼽아줍니다.
이 Errors 오브젝트에 에러 처리 결과를 넣어주면 JSP `<form>` 태그에서 에러 결과를 출력할 수 있습니다.

rejectValue 에서 첫번째 인자는 필드명, 두번째 인자는 에러 코드명입니다.
실제 에러 메시지는 에러 코드를 가지고 messages.properties 에서 가져옵니다.

    password.wrong=잘못된 비밀번호입니다.
    userName.notFilled=필명이 입력되지 않았습니다.

messages.properties 파일 예입니다.  
위치는 Maven 표준 디렉토리 기준 src/main/resources 하위 마음에 드는 곳에 넣어주시면 됩니다.

새로운 파일들이 등장했으니 연결 정의를 스프링에 해놓아야 합니다.

    <bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
      <property name="basename" value="classpath:messages"/>
      <property name="defaultEncoding" value="UTF-8"/>
    </bean>

토비 스프링3 책에는 ResourceBundleMessageSource 를 쓰라고 되어 있는데 이러면 인코딩 문제가 발생합니다.
ResourceBundleMessageSource 쓰면 토비3 책에서 처럼 한글을 /uXXXX 식으로 바까서 넣어야하는데 이건 아니죠. =,=

다행히 ReloadableResourceBundleMessageSource 에 인코딩 지정능력이 있어서 저걸 쓰면됩니다.
Reloadable 이라고 되어있는데 기본적으로 리로딩 기능은 죽어있으니 다른 걱정하지 않아도 됩니다.

    <form:form commandName="postForm" enctype="multipart/form-data">
      <div class="input"><form:errors path="*" cssClass="error-summary"/></div>
    
      <div class="label"><form:label path="post.userName" cssErrorClass="error">필명</form:label></div>
      <div class="input"><form:input path="post.userName" cssClass="tb" maxlength="32"/></div>

에러 출력은 `<form:errors>` 태그로 모아서 오류 내용 뿌리고,
`<forrm:label>` 태그에 `cssErrorClass` 넣어서 오류 나면 빨갛게 표시하는 정도면 무난할 것 같군요.


{:class="go-to-index"}
[Java Spring](index)
