---
layout: post
title:  "svn merge 쉽게 하기."
date:   2016-06-03 10:20:00 +0900
categories: jekyll update
---
SVN을 운영할 때 가장 어려워하는 부분이 merge일 것이라고 생각합니다.
merge 부분 설명을 봐도 정확하게 기술되어 있지 않아서 항상 trunk에 머지했다고 생각했는데 
사실은 반대로 되는 경우가 많기도 하고, conflict가 날 상황이 아닌데도 불구하고 conflict가 
나기도 합니다.

```bash
# --dry-run은 preview merge 기능을 제공함.
#
# branch 디렉토리에 trunk를 머지하고자 할 때
#
svn merge --dry-run ^/trunk
svn merge ^/trunk

# trunk 디렉토리에 특정 branch를 머지하고자 할 때
#
svn merge --dry-run ^/branches/mybranchname
svn merge ^/branches/mybranchname

# conflict가 발생한 경우 추가적인 옵션으로 해결이 가능할 때
#
svn merge --dry-run --reintegrate ^/branches/mybranchname
svn merge --reintegrate ^/branches/mybranchname

```

다행히도 좋은 블로그를 발견해서 여기에 간단하게 블로그에 포함된 내용을 적어둡니다.

* [SVN branch and merge 쉽게 활용하기 #2](http://asbear.tistory.com/72)
* [SVN trunk 변경사항 되돌리기 (SVN Rollback)](http://asbear.tistory.com/74)
