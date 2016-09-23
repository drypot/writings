---
title: Configuration
---

### 사용자명 등록

Git 를 사용하기에 앞서 사용자명을 등록해야한다.
사용자명은 커밋에 기록된다.

    $ git config --global user.name “drypot”
    $ git config --global user.email “drypot@gmail.com”

### Config Files

설정파일은 3 단계로 존재한다.

    ./.git/config # default
    ~/.gitconfig # with the --global option
    /etc/gitconfig # with the --system option

설정 확인.

    $ git config -l

설정 삭제.

    $ git config --unset --global user.email


{:class="go-to-index"}
[Git ABC](index)