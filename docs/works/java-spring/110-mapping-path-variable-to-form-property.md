---
title: Mapping Path Variable to Form Property
---

2010-10-26

패스 변수를 폼 프로퍼티에 매핑할 수 있을까요? 그러니까,

    @RequestMapping(value = "/post/{postThreadId}", method = RequestMethod.GET)
    @ResponseBody
    public String paramTest(PostParams postParams) {
      return postParams.toString();
    }

`PostParams` 가 `setPostThreadId` 세터를 가지고 있을 때 `postThreadId` Path Variable 을
`postParams` 에 매핑할 수 있을까요?

물어봤는데, 없다고합니다. =,=

현재로서는 `@PathVariable int postThreadId` 로 받아서 수동 set 만이 가능.


{:class="go-to-index"}
[Java Spring](index)
