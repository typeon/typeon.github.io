---
layout: post
title:  "git revert"
date:   2016-08-30 10:20:00 +0900
categories: git
---

## 되돌리기.

### commit 되돌리기.

-> 이건 아직 이해가 안되서 추가하지 않는다.

### 파일을 unstage로 바꾸기.

```
$ git reset HEAD source.rb

```

### modified 파일 되돌리기.

```
$ git checkout -- source.rb
```

Original source: [https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0)
