---
title: Reference
---

### Reference

    .git/<ref name>
    .git/refs/<ref name>
    .git/refs/tags/<ref name>
    .git/refs/heads/<ref name>
    .git/refs/remotes/<ref name>
    .git/refs/remotes/<ref name>/HEAD

리포지터리에 저장되는 파일은 모두 40 자리 이름을 가지는데 이를 직접다루는 것은 불편하므로
해쉬 이름을 대신하는 레퍼런스를 (Symbolic reference) 사용할 수 있다.

레퍼런스는 태그와 브랜치등 몇 가지 상위 개념을 구현하는데 사용된다.
태그와 브랜치가 특정 커밋을 가리키는 레퍼런스라는 점에선 같으나
브랜치는 계속되는 개발 진행 상황을 관리하기 위해 사용하고
태그는 특정 커밋에 이름을 붙여두기 위해 사용한다.

레퍼런스는 위의 모든 디렉토리들에서 검색되기 때문에 긴 이름을 쓸 수도 있고 짧게 줄여 쓸 수도 있다.
예로, `dev` 는 `refs/heads/dev` 의 짧은 이름이 될 수 있다.
`origin/master` 는 `refs/remotes/origin/master` 의 짧은 이름이 될 수 있다.


### Root Reference

`.git/` 디렉토리에는 맨 처음 규칙을 만족시키는 `HEAD`, `ORIG_HEAD`, `FETCH_HEAD`, `MERGE_HEAD` 등의 특별한 레퍼런스 파일들을 볼 수 있다. 이 레퍼런스들은 Git 가 특별히 관리한다.

- HEAD:
  현재 브랜치의 최신 커밋을 가리킨다.

- ORIG_HEAD:
  `merge` 나 `reset` 명령은 HEAD 를 ORIG_HEAD 라는 이름으로 백업해 둔다.

- FETCH_HEAD:
  `git fetch` 명령은 fetch 한 모든 브랜치들의 HEAD 를 `.git/FETCH_HEAD` 에 저장한다.

- MERGE_HEAD:
  merge 중 merge 하고 있는 다른 브랜치의 HEAD 가 MERGE_HEAD 에 저장된다.


### Relative Commit Names

브랜치 이름의 끝에 특수 문자를 붙여서 커밋을 상대 지정할 수 있다.
`master^` 는 master 브랜치 헤드의 부모 커밋을 가리킨다. `master~1` 도 같은 의미다.
`master^^` 는 master 브랜치 헤드의 부모 부모 커밋을 가리킨다. `master~2` 도 같은 의미다.
머지는 여러 부모를 갖는데 머지로 생성된 `C` 라는 커밋이 있을 때
`C^1` 은 첫 번째 부모 커밋, `C^2` 는 두 번째 부모 커밋을 가리킨다.


### Show Reference

리포지터리의 레퍼런스 목록을 본다.

    $ git show-ref

리모트 리포지터리의 레퍼런스 목록을 본다.

    $ git ls-remote origin

레퍼런스 이름에 대한 커밋 ID 를 찾을 수 있다.

    $ git rev-parse master


{:class="go-to-index"}
[Git ABC](index)
