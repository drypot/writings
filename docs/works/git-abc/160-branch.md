---
title: Branch
---

부모와 자식 커밋들은 줄줄이 사탕으로 연결되어 있는데 이 줄기를 브랜치라 한다.

Git 에선 브랜치 생성이 매우 가벼운 작업이다.
작은 기능을 추가할 경우에도 브랜치를 분기한다.
성공하면 메인에 머지하고 실패하면 브랜치를 버리는 방식으로 작업하는 경우가 많다.
하루에도 브랜치 분기와 머지가 여러 번 발생한다.

브랜치는 레퍼런스로 구현된다.
master 브랜치를 찍어보면 실체는 커밋이라는 것을 알 수 있다.

    $ git show master
    commit 97f881b9ffa8986652ca31ed5a8efceb4115aa0d
    ...

여러 브랜치가 존재할 수 있으므로 현재 브랜치를 가리키는 레퍼런스가 필요한데 이것이 HEAD 다.
현재 브랜치가 master 일 경우 HEAD 는 master 를 가리키고 있다.

    $ cat .git/HEAD
    ref: refs/heads/master

HEAD 를 찍어보면 master 와 같은 커밋을 출력한다는 것을 확인할 수 있다.

    $ git show HEAD
    commit 97f881b9ffa8986652ca31ed5a8efceb4115aa0d
    ...


### 브랜치 목록

로컬 브랜치 목록을 보여준다. '*' 는 현재 브랜치를 의미한다.

    $ git branch
    * master

로컬 브랜치와 리모트 브랜치를 모두 보여준다.

    $ git branch -a
    * master
    remotes/origin/master

로컬 브랜치들의 히스토리를 보여준다.

    $ git show-branch --more=10

로컬 브랜치와 리모트 브랜치의 히스토리를 보여준다.

    $ git show-branch -a


### 브랜치 생성

현재 브랜치가 master 일 때

    $git branch
    * master

develop 브랜치를 생성할 수 있다.

    $ git branch develop

이 명령은 브랜치를 생성하기만 할 뿐 현재 브랜치를 바꾸진 않는다.

    $git branch
    * master
    develop


### 브랜치 삭제

브랜치 삭제.

    $ git branch -d <commit>

이 명령은 브랜치 레퍼런스만 삭제한다.
브랜치에 연결되어 있던 커밋들은 그대로 방치되다가 GC 할 때 삭제된다.

    $ git gc


### 브랜치 변경

`git checkout` 을 사용한다.


{:class="go-to-index"}
[Git ABC](index)

