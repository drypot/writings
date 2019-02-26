---
title: Remote
---

Git 는 다수의 리모트 리포지터리들을 가질 수 있다.


### 리모트 리포지터리 등록

리모트 리포지터리를 추가로 연결하려면 먼저 `git remote` 로 등록한다.

    $ git remote add origin https://github.com/drypot/about.git

리모트 이름은 origin 을 사용하는 것이 관행이다.

리모트 정보는 .git/config 파일에 저장된다.

    $ cat .git/config
    ...
    [remote "origin"]
    url = https://github.com/drypot/writings.git
    fetch = +refs/heads/*:refs/remotes/origin/*

리모트 목록을 출력한다.

    $ git remote
    $ git remote -v

리모트에 관한 정보를 표시한다.

    $ git remote show origin

리모트의 이름을 바꾼다

    $ git remote rename origin origin2

리모트를 삭제한다.

    $ git remote rm origin

리모트 리포지터리에서 삭제된 브랜치들을 로컬에서도 삭제한다.

    $ git remote prune origin


### 리모트 브랜치

리모트 브랜치는 리모트 리포지터리의 상태를 반영하기 위한 로컬 브랜치다.

origin 리포지터리의 master 브랜치에 대한 리모트 브랜치는 origin/master 가 된다.

    $ git log origin/master


### 트래킹 브랜치

`push`, `pull` 에 사용될 리모트 브랜치 정보를 가지고 있는 브랜치를 트래킹 브랜치라한다.
리모트 정보와 함께 트래킹 정보도 `.git/config` 에 저장된다.

    [branch “master”]
    remote = origin
    merge = refs/heads/master

트래킹 정보는 리모트 브랜치에서 `checkout` 하거나

    $ git checkout -b master origin/master

첫 `push` 시 `-u` 옵션으로 생성할 수 있다.

    $ git push -u origin master


### 리모트 데이터 가져오기

리모트에 업데이트된 커밋들을 가져올 수 있다.
트래킹 브랜치에 머지하진 않는다.

    $ git fetch origin

등록된 모든 리모트의 업데이트 내용을 한번에 가져올 수 있다.

    $ git fetch --all

다음 명령도 비슷한 작업을 한다.

    $ git remote update


### 리모트 브랜치 머지

`fetch` 한 내용은 `merge` 해야 로컬 브랜치에 반영된다.

    $ git fetch
    $ git merge FETCH_HEAD

`fetch` 명령이 가져온 레퍼런스들을 `FETCH_HEAD` 에 저장한다는 점을 이용하였다.
하지만 실전에선 `fetch` 와 `merge` 를 합친 `pull` 을 사용한다.

    $ git pull

`merge` 중 충돌이 나면 수작업후 커밋을 해야한다.
`merge` 전 상태로 되돌아갈 수도 있다.

    $ git reset --hard ORIG_HEAD


### 리모트로 데이터 전송

로컬 리포지터리의 업데이트를 리모트에 `push` 할 수 있다.

    $ git push

리모트 브랜치에서 checkout 하지 않고 로컬에서 처음 만들어진 브랜치라면 트래킹 정보를 한번 세팅해주어야 한다.

    $ git push -u origin master

리모트에 변화가 있었다면 먼저 `pull` 한 후 `push` 해야한다.


### 리모트 리포지터리 브랜치 삭제

리모트 리포지터리의 브랜치를 삭제할 수 있다.

    $ git push origin :feature


### 리모트 리포지터리 클론

리포지터리를 클론 명령은

    $ git clone https://github.com/drypot/about.git

아래 절차를 수작업하는 것과 비슷하다.

    $ mkdir about
    $ cd about
    $ git init
    $ git remote add origin https://github.com/drypot/about.git
    $ git fetch origin
    $ git checkout -b master origin/master


{:class="go-to-index"}
[Git ABC](index)
