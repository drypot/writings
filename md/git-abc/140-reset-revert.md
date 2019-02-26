---
title: Reset & Revert
---

### git reset

인덱스를 리셋하여 `git add` 를 모두 취소할 수 있다.

    $ git reset

특정 파일의 인덱싱만 취소할 수 있다.

    $ git reset -- readme.md

인덱스뿐만 아니라 워킹 디렉토리까지 리셋할 수 있다.

    $ git reset --hard

최근 3 회의 커밋 기록을 삭제할 수 있다.

    $ git reset --hard master~3

마지막 커밋 이전 커밋으로 돌아간다. 위킹디렉토리 내용은 변경하지 않는다.

    $ git reset --soft HEAD~1

Reset 은 가능하면 최신 커밋으로 복구할 때만 사용하고
커밋을 되돌릴 때는 Reset 대신 Revert 를 쓰는 것이 안전하다.


### git revert

마지막 커밋을 복구하는 새로운 커밋을 추가한다.

    $ git revert --no-edit master


### git clean

트래킹 되지 않고 있는 위킹 디렉토리 파일들을 모두 삭제.

    $ git clean -f


{:class="go-to-index"}
[Git ABC](index)

