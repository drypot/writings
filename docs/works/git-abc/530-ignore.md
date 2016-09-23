---
title: Ignore
---

### .gitignore

컴파일러 중간 파일등 버전관리가 필요가 없는 파일들은 `.gitignore` 파일에서 지정한다.

서브디렉토리의 `.gitignore` 는 상위 디렉토리의 `.gitignore`을 오버라이딩한다.

`.gitignore` 파일은 자동으로 `git add` 되지 않으니 수동으로 넣어야한다.

.gitignore 예.

    # 이 줄은 코멘트.
    # 패턴 앞에 ! 을 붙이면 not 의 의미

    # 파일 이름만 써놓으면 모든 디렉토리에서 해당 파일을 무시한다.
    ignore-me-everywhere.txt

    # OSX 에서는 폴터 관리 파일도 무시하자.
    .DS_Store

    # 끝이 / 로 끝나면 해당 디렉토리 전체를 무시한다.
    ignore-dir/

    # 와일드카드를 쓸 수도 있다.
    debug/32bit/*.o
    *.[oa]
    tmp/**/*
    log/*

    # 개발시 로컬 머신에서만 의미있는 설정 파일들도 무시한다
    /config/database.yml


### .git/info/exclude

로컬 리포지터리에만 적용하고 싶은 예외 규정은 `.git/info/exclude` 에서 설정한다.
리포지터리 클론시 복사되지 않는다.


{:class="go-to-index"}
[Git ABC](index)
