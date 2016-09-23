---
title: Show
---

### git show

커밋의 상세 정보를 볼 수 있다.

    $ git show
    $ git show 1e9321f

파일 내용을 볼 수 있다.

    $ git show origin/master:Makefile
    $ git show HEAD~4:src/main.c


### git ls-tree

트리를 볼 수 있다.

    $ git ls-tree master


### git cat-file

파일을 덤프할 수 있다.

    $ git cat-file -p 1e9321f2929c8b167e1c25f119e14e912949b71a
    $ git cat-file -p 1e9321f2


### blame

코드의 각 행이 언제 수정되었는지 확인

    $ git blame path


### git diff

변경된 파일 목록.

    $ git diff --name-only HEAD~1 HEAD


{:class="go-to-index"}
[Git ABC](index)

