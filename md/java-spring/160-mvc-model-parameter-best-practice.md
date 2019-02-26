---
title: MVC Model Parameter BestPractice
---

2010-11-04

책에서 보셨을 것인데요 컨트롤러에서 모델을 뷰로 넘기는데 관여하는 타입들이 여러가지 있습니다.
`ModelAndView`, `Model`, `Map`, `ModelMap` 등이 있습니다.
비슷비슷하게 보이죠? =,=

일단 `ModelAndView` 는 가장 기초기능을 하는 오브젝트이고
실제로 스프링 내부에서는 저 오브젝트가 생성되서 왔다갔다 하지만
Spring MVC 2.X 대에서 주로 쓰던 방식이니 직접 사용하지 않으셔도 될 것 같습니다.

`Map` 을 쌩으로 써도 되는데 타입 유지가 안 되고 너무 평범해 보이니 왠지 기분이 나쁩니다.
이것도 제껴 둡니다.

`ModelMap` 은 쌩 `Map` 을 쓰면 `Model` 인터페이스를 쓰는 것보다 좀 불편하니 그것을 돕기 위해 나온 놈입니다.
이름은 비슷하지만 `Model` 인터페이스와는 근본이 피 한 방울 섞지 않은 놈입니다. 까보면 `Map`.
기분이 나쁘니 이것도 뺍니다.

나머지 `Model` 이 남았습니다.
네 이걸 쓰시라 말씀드리려고 서론이 이렇게. =,=.
어, 그런데 보니 `Model` 은 인터페이스라 컨트롤러에서 오브젝트를 만들 수가 없습니다.
오브젝트를 만들 수도 없는데 어떻게 값을 넣을 것이며, 어떻게 뷰로 보내나요?

메서드 내에서 오브젝트를 만드는 것은 옛날 취향이라고 전에 말씀 드린 적이 (없던가요 =,=).
테스트도 어렵게 만들고 유연성도 떨어뜨리니 가능하면 오브젝트 생성은 인자 전달 과정에서만 만들어서
메서드 내부에서 (특히 시작 시점에 주로) 쌩뚱맞게 new 어쩌구 하는 일은 이제 가능한 삼가야합니다.
(이 취향은 펑셔널 프로그래밍에서 온 것이기도 한데 따르면 좋은 느낌이 들게 됩니다)

장황해 지겠지만 이부분을 좀 길게 적으면,

    void method() {
      Apple apple = new apple();
      Banana banana = new Banana();
      // code using apple and banana 
    }

이런 식으로 하지 마시고,

    void method(Apple apple, Banana banana) {
      // code using apple and banana
    }

이런 식으로 하신 후, 콜하는 부분에서 오브젝트를 만들어 던지란 말이지요.

   method(new Apple(), new Banana);

이 스타일을 컨트롤러에 가져오면
Model 오브젝트도 컨트롤러 핸들러 메서드에서 만들어 쓰는게 아니고 받는 것이 좋다는 말입니다.
그래야 테스트도 쉽고, 테스트를 활용하지 않더라도 우리 인생이 쉬워집니다.
스프링이 던져주는거 그냥 쓰면 되니까요.

새글쓰기 메서드 경우를 예로 보면.

    @RequestMapping(value = "/post/new", method = RequestMethod.GET)
    public String newPost(PostParams postParams, Model model, HttpServletRequest request) {
      PostForm postForm = new PostForm();
      model.addAttribute(postForm);
      return "post/new";
    }

새로 생성한 빈 postForm 오브젝트를 뷰로 던지기 위해 model 오브젝트를 통하게 되는데
이것을 인자로 스프링으로 부터 주입받고 있습니다.
네, 이렇게 Model 타입 오브젝트를 전달 받는 것이
M 으로 시작하는 4 인방을 다루는 방법중에 가장 괜찬은 정석이라 할 수 있습니다.
하지만 여기서 더 나가갈 수 있습니다.

난 빈 PostForm 을 던지고 싶은데 이걸 그냥 스프링보러 알아서 하라고 하면 더 좋지 않겠습니까?
가능하고요, 위에서 PostForm 을 생성하고 이걸 또 Model 에 넣는 구분이 싹 필요없어집니다.
이렇게.

    @RequestMapping(value = "/post/new", method = RequestMethod.GET)
    public String newPost(PostParams postParams, PostForm postForm, HttpServletRequest request) {
      return "post/new";
    }

보시면 PostForm 을 바로 주입받고 있습니다.
스프링은 인자로 정의된 오브젝트를 일단 다 만들고 보니 빈 오브젝트가 낑겨 있을 겁니다.
그리고 저렇게 인자에 선언된 오브젝트는 자동으로 Model 에도 넣어 두니까
우리가 수작업으로 낑굴 필요도 없겠습니다.

그러니 결론은 사실 저위에 M 4 인방은 전부 우리 눈에 띄일 필요가 없는 놈이었습니다.
만들고 계신 코드에 M... 이 보이면 수정해서 더 간단히 하실 수 있습니다.


{:class="go-to-index"}
[Java Spring](index)
