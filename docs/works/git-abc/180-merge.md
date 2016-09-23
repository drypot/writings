---
title: Merge
---

### git merge

다른 브랜치를 현재 브랜치에 합칠 수 있다.

현재 브랜치가 master 라면

    $ git checkout master

develop 브랜치를 master 에 머지할 수 있다.

    $ git merge develop

머지 과정에서 충돌이 없다면 자동으로 커밋이 생성된다.


### 충돌

충돌이 있을 경우 문제를 수작업으로 해결한 후 커밋해야 한다.

충돌한 파일들의 목록을 보려면.

    $ git status

충돌한 머지를 버리고 워킹 디렉토리를 최종 커밋으로 리셋하려면.

    $ git reset --hard

머지에 의해 발생한 커밋을 버리고 머지 이전 커밋으로 되돌리려면.

    $ git reset --hard ORIG_HEAD

머지 명령이 HEAD 를 ORIG_HEAD 에 백업하는 것을 이용하였다.


### 머지 정책

머지 실행시 Git 는 머지 정책을 선택한다.

- Already up-to-date:
  머지하려는 브랜치의 헤드가 과거에 이미 머지되어 있는 경우에는 머지를 중지한다.

- Fast-forward:
  분기해서 수정한 브랜치를 다시 원래 브랜치에 머지하려는 경우 머지 커밋 생성없이 브랜치 레퍼런스만 업데이트한다.
  FF 가 가능한 상황이라면 자동으로 FF 선택되는데 이를 막고 머지 커밋을 강제 생성할 수 있다.
  `$ git merge --no-ff develop`

- Resolve:
  평범한 두 브랜치 머지다.
  두 브랜치의 분기점을 찾아서 그 이후 발생한 소스 브랜치의 변경을 타겟 브랜치에 적용한다.

- Recursive:
  Resolve 의 경우와 흡사하나 머지 베이스가 하나 이상일 경우 머지 베이스들을 일단 머지해서 임시 머지 베이스를 만든 후 메인 머지에 사용한다.

- Octopus:
  머지할 브랜치가 세 개 이상일 때는 두 개씩 Recursive 머지를 순차적으로 시도한다.


{:class="go-to-index"}
[Git ABC](index)

