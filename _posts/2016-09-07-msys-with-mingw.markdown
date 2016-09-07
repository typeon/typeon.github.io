---
layout: post
title:  "using MSYS with MinGW"
date:   2016-09-07 10:20:00 +0900
categories: msys
---


### using MSYS with MinGW

To install 3rd party library and applications which uses the autotools build system the following commands are often used.

```
./configure --prefix=/mingw
make
make install
```

Installing to "/usr/local" should be avoided, since the MinGW compiler won't look there by default.


Original Source: [http://www.mingw.org/wiki/msys](http://www.mingw.org/wiki/msys)
