---
title: MVC Composite Form Object
---

2010-11-01

Spring MVC Form 처리 기본 시나리오에서는 하나의 코멘드 오브젝트를 가정하고 있습니다.
코멘드 오브젝트 이름은 `<form:form commandName="postForm"/>` 식으로 지정하게 되어 있고
개개의 input tag 에서 별도 지정할 수가 없습니다.

문제는 업무처리를 하다보면 순수한 Domain Object 의 필드외에 곁다리 필드들이 필요하거나
User 와 Company 처럼 두개 이상의 Domain Object 를 하나의 폼으로 처리해야할 일들이 꼭 발생한다는 겁니다.

포럼 보니 이와 같은 문제에 짜증내는 사람이 많고
유겐휄러가 스프링 3 에서 이 문제 해결해주겠다고 2 년전 포스팅한 글이 있는데 실제로 고쳐진 것은 없습니다.
로드맵을 봐도 고칠 생각이 없는 듯. =,=

이럴 때는 순수 Domain Object 를 감싸는 Form 전용 오브젝트를 만들고
추가 필드나 추가 오브젝트를 링크해서 씁니다.

    public class PostForm {
      private PostThread postThread;
      private Post post;
    
      // getters + setters
    }

폼 오브젝트 정의는 별거 없습니다.
폼에 표시하고 싶은 오브젝트를 자바 빈 규약에 따라 다 연결하면 됩니다.

    @RequestMapping(value = "/post/new", method = RequestMethod.POST)
    public String newPost(PostForm postForm)
    { }

컨트롤러 코드도 특이한 것이 없습니다.
폼 오브젝트를 통채로 받으면, 안에 서브 오브젝트가 자동으로 채워져 있습니다.

    <form:form commandName="postForm" enctype="multipart/form-data">
      <div class="input"><form:errors path="*" cssClass="error-summary"/></div>
    
      <div class="label"><form:label path="post.userName" cssErrorClass="error">필명</form:label></div>
      <div class="input"><form:input path="post.userName" cssClass="tb" maxlength="32"/></div>
    
      <div class="label"><form:label path="postThread.title" cssErrorClass="error">제목</form:label></div>
      <div class="input"><form:input path="postThread.title" cssClass="tb" maxlength="128"/></div>
    </form:form>

JSP 페이지에서 commandName 은 위와 같이 폼 오브젝트에 대해 지정해주시고,
필드 입력 태그 path 에는 서브 오브젝트 이름의 프리픽스를 모두 달아주시면 됩니다.

필드명에 dot 가 들어가서 JavaScript, JQuery 코드를 다룰 때 살짝 주의해야합니다.
아래 처럼 dot 를 \\ 나 쿼트로 escape 해줍니다.

    $("#post\\.userName")
    $("input[name!='post.userName']")


{:class="go-to-index"}
[Java Spring](index)
