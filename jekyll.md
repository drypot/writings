# Jekyll

github 리포지터에서 사용하는 jekyll 의 실제 폴더는 /docs 다.

OS 가 깨끗한 환경에 새로 포크했다면 이 리포지터리를 포크했다면 루비와 젬들이 없을 것이다.

루비 설치.

    brew install ruby

번들러 설치.

    gem install bundler

번들러로 젬 설치.

    cd docs
    bundle install

초기 설치 대신 젬 업데이트 하려면,

    bundle update

지킬 실행법 1, 루트로 돌아와서,

    cd ..
    bin/jekyll-run

지킬 실행법 1, docs 에서 그냥,

    bundle exec jekyll serve

지킬 실행 오류나면, 지킬 사이트 새로 만들어서 GemFile, GemFile.lock 복사해 온다.

    jekyll new "tmp_site"

