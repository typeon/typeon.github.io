---
layout: post
title:  "explicit constructor in C++"
date:   2016-09-01 14:15:00 +0900
categories: c++
---

### explicit

설명이 아주 쉽게 되어 있어서 완전 이해했다. 
``AAA a = 10; AAA a(10);``에서 `AAA`의 explicit constructor ``explicit AAA(int a)``가 정의되면 
``a = 10;``에서 에러가 난다. 이럴 때 ``AAA a(10)``으로만 호출하라는 의미이다. 

### volatile

변수를 register 등에 적재하는 최적화를 금지시키는 keyword이다.
주로 사용하는 경우는 hardware 접근, 멀티쓰레드 메모리 변수, 인터럽트 루틴이다. 
**register keyword의 반대라고 생각하자.**

Original source: [http://pacs.tistory.com/entry/C-Explicit%EA%B3%BC-Mutable](http://pacs.tistory.com/entry/C-Explicit%EA%B3%BC-Mutable),
[http://skyul.tistory.com/337](http://skyul.tistory.com/337)
