---
title: Repository
---

### 워킹 디렉토리

평소 우리가 코딩하는 그곳이다.

### 리포지터리

'git init’ 명령을 실행하면 .git 디렉토리가 생성된다.
리포지터리라 부르고 Git 의 모든 데이터가 저장된다. (오브젝트, 인덱스, 레퍼런스등)

### 오브젝트 스토어

    .git/objects/*

기록에 관해 상상할 수 있는 모든 것은 여기에 저장된다. (파일, 트리 노드, 커밋등)
오브젝트 스토어 구조는 리포지터리를 효율적으로 클론할 수 있도록 만들어졌다.
오브젝트 스토어에 저장되는 오브젝트는 딱 네 종류다.

- Blob
  파일은 블롭으로 저장된다.
  파일 데이터만 저장된다. 파일 이름은 저장되지 않는다.
  Git 는 diff 를 저장하지 않는다. 파일 전체를 저장한다.
  diff 는 필요할 때마다 계산해서 표시한다.
  블롭은 데이터 구조의 최하단에 있으며 다른 오브젝트에 대한 링크를 갖지 않는다.

- Tree
  블롭의 해쉬, 패스, 기타 메타 정보가 기록된다.
  트리 오브젝트는 다른 트리나 블롭에 대한 링크를 갖는다.

- Commit
  커밋에는 커밋 메타 데이터와 루트 트리의 링크가 저장된다.
  이전 커밋에 대한 링크를 가진다.
  머지의 결과로 만들어진 커밋은 여러 부모 커밋을 갖는다.
  서로 다른 커밋 오브젝트가 같은 트리에 대한 링크를 가질 수 있다.

- Tag
  주로 커밋에 유용한 별칭을 붙여두기 위해 사용.

### SHA1 파일명

리포지터리는 파일내용의 SHA1 해쉬를 파일이름으로 사용한다.

    $ git ls-tree master
    100644 blob 8baef1b4abc478178b004d62031cf7fe6db6f903 readme.md
    100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 three.md
    100644 blob 2abcdfd5c0c7573ff9bb3e8cdb75c93590aa0460 two.md

    $ git show 8baef1
    ...

    $ git log -1
    commit 53659d1e966f312cdc37addb06a7e2e20e33615f
    Author: drypot <drypot@gmail.com>
    ...

    $ git show 53659d1
    ...

SHA1 해쉬는 20 byte (40 문자) 로 구성되는 16 진수 숫자열이다.
블롭, 트리, 커밋, 태그 파일 모두 SHA1 해쉬를 이름으로 갖는다.
SHA1 해쉬가 같은 파일들은 한 벌만 저장된다.

이름 대신 SHA1 해쉬를 쓰므로 커밋이 겹겹이 쌓인 상태에서도 SHA1 해쉬만 가지고 특정 버전의 파일을 쉽게 가리킬 수 있다.
트리 오브젝트의 해쉬가 같다면 서브 디렉토리 모든 파일을 비교하지 않고도 두 디렉토리 내용이 같다는 보장을 얻을 수 있다.
해쉬만으로 동일여부를 판단하므로 매우 큰 오브젝트들의 존재 유무를 데이터 전송없이 알아낼 수 있다.

내용이 다른 파일간 SHA1 해쉬가 충돌할 가능성은 있다. 하지만 엄청나게 낮다.
충돌나면 어떻게 되는지에 대한 말은 안 보인다. =,=

### 새 로컬 리포지터리 생성

Git 를 사용하려면 제일 먼저 리포지터리를 생성해야 한다.
혼자 VCS 기능을 사용하는데 적합하다.

    $ cd my-project
    $ git init

리포지터리 디렉토리가 생긴 것을 확인할 수 있다.

    $ ls -d .git
    .git

### 새 GitHub 리포지터리 생성

<https://github.com>{:target="_blank"} 에 리포지터리를 생성하고 내 코드를 공개할 수 있다.

리모트 리포지터리를 사용하려면 리모트와 로컬 리포지터리를 모두 생성한 후 서로를 연결해야한다.
리모트 리포지터리를 먼저 생성하고 내 컴퓨터에 클론을 만드는 방식이 조금 쉽다.

위 사이트의 안내에 따라 일단 리모트 리포지터리를 생성한다.
생성된 리포지터리에 가면 `https://github.com/drypot/my-project.git` 처럼 생긴 리포지터리 주소를 발견할 수 있다.

my-project 라는 리모트 리포지터리를 생성했다면 아래 명령으로 로컬에 클론을 생성할 수 있다.

    $ git clone https://github.com/drypot/my-project.git
    $ cd my-project

새로 만든 리모트 리포지터리에 무엇인가 당장 보내 보고 싶다면 일단 아무 파일이나 만들어서

    $ touch readme.md

만든 파일을 인덱스에 등록하고

    $ git add readme.md

커밋한 다음

    $ git commit -m "first commit"

리모트에 푸쉬한다.

    $ git push

리모트 리포지터리에 가보면 `readme.md` 가 등록되어 있을 것이다.


### 기존 로컬 리포지터리를 GitHub 에 연결

이미 존재하는 로컬 리포지터리를 리모트 리포지터리에 연결할 수 있다.

로컬에 리포지터리가 생성되어있다고 가정하고

    $ cd my-project
    $ git init

사이트의 안내에 따라 `my-project` 라는 이름의 리모트 리포지터리를 생성했다면 아래처럼 로컬에 리모트를 등록할 수 있다.
`origin` 은 관행처럼 사용되는 리모트 이름이다.

    $ git remote add origin https://github.com/drypot/my-project.git

이제 로컬 리포지터리 내용을 리모트에 푸쉬할 수 있는데 처음 한번은 `-u` 옵션으로 전송할 브랜치를 설정해야 한다.
master 는 브랜치명인데 리포지터리 생성시 기본으로 만들어지므로 당장 전송할 수 있다.

    $ git push -u origin master

다음 부터는 옵션 없이 푸쉬할 수 있다.

    $ git push


### 초기화된 리포지터리의 .git/config 예

    [core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
        ignorecase = true

    [remote "origin"]
        url = https://github.com/drypot/raysoda.git
        fetch = +refs/heads/*:refs/remotes/origin/*

    [branch "master"]
        remote = origin
        merge = refs/heads/master


{:class="go-to-index"}
[Git ABC](index)