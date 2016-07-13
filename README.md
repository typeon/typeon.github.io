## typeon.github.io
Typeon Mini Blogging Service

## 설치 및 테스트 방법.
```
# gem을 설치.
sudo gem install jekyll

# 테스트 실행.
cd typeon.github.io
jekyll serve -w
```

## 블로그 작성하기.

다음을 참고합니다. https://jekyllrb.com/docs/home/

* _post 디렉토리 내에 yyyy-mm-dd-title.markdown 파일명으로 새로운 파일을 생성합니다.
* `jekyll serve -w` 명령을 실행하고 `http://localhost:4000`에 접속합니다.
* 신규로 만든 파일을 확인하고 이상 여부를 파악합니다.
* `git add` 명령으로 git에 추가하고 `git push`해서 `github.com`에 변경 내용을 반영합니다.