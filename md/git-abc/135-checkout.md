---
title: Check Out
---

### git checkout

develop 브랜치를 로드한다.

    $ git checkout develop

    $ git branch
    master
    * develop

master 브랜치로 돌아온다.

    $ git checkout master

`git checkout` 은 지정 커밋으로 HEAD 을 변경한다.
워킹 디렉토리에 커밋 파일들을 로드한다.

커밋하지 않은 파일들은 보존된다.
브랜치 이동후 새 브랜치에 커밋할 수 있다.

브랜치 관령 설명은 브랜치 노트에.


### git checkout file

특정 파일만 로드할 수도 있다.

    $ git checkout -- readme.md

특정 커밋 파일로 복구.

    $ git checkout master~3 -- readme.md


### git checkout + 브랜치 생성

브랜치 생성과 변경을 한번에 할 수 있다.

    $ git checkout -b hotfix-101

위 명령은 아래 두 명령과 의미가 같다.

    $ git branch hotfix-101
    $ git checkout hotfix-101


### Detached HEAD

일반적으로 HEAD 는 브랜치 헤드를 가리키고 있는데 비헤드 커밋을 채크아웃할 수 있다.
이 상태를 Detached HEAD 라고 한다.

    $ git checkout master~9

Detached HEAD 상태에서도 커밋할 수는 있는데

    $ git commit -m "...."

이렇게 생성된 커밋은 아무 브랜치에도 속하지 않기 때문에
브랜치를 만들어 연결해 주지 않으면 GC 과정에서 삭제된다.

비브랜치 커밋에 브랜치를 연결할 수 있다.

    $ git checkout -b new-branch

`checkout` 하지 않고 브랜치만 생성해서 연결할 수도 있는데

    $ git branch new-branch

이렇게 하면 HEAD 는 아직도 Detached 상태이므로 다음 커밋은 다시 비브랜치 커밋이 된다.


{:class="go-to-index"}
[Git ABC](index)

