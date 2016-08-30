---
layout: post
title:  "cancel modified files in git clone."
date:   2016-07-22 16:30:00 +0900
categories: git
---

### reset 처리.

* `git reset --hard HEAD`: 워킹트리 전체를 마지막 커밋 상태로 되돌림. 마지막 커밋이후의 워킹트리와 index의 수정사항 모두 사라짐.
                                  (변경을 커밋하지 않았다면 유용)
* `git checkout HEAD .`: ??? 워킹트리의 모든 수정된 파일의 내용을 HEAD로 원복.
* `git checkout -f`: 변경된 파일들을 HEAD로 모두 원복(아직 커밋하지 않은 워킹트리와 index 의 수정사항 모두 사라짐. 신규추가 파일 제외)

Original source: [http://ecogeo.tistory.com/276](http://ecogeo.tistory.com/276)
