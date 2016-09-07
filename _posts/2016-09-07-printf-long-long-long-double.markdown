---
layout: post
title:  "using MSYS with MinGW"
date:   2016-09-07 10:20:00 +0900
categories: c++
---


### long long, long double printf format

* long long -> ``%lld``
* long double -> ``%ldx``
* unsigned long long -> ``%llu``

```
#include <stdio.h>

int main() {
    long ln = 123456789L;
    long long lln = 1234567890123456789LL;
    double dx = 1.234567890123456789;
    long double ldx = 1.234567890123456789L;
    printf("ln = %d\n", ln);
    printf("ln = %ld\n", ln);
    printf("lln = %lld\n", lln);
    printf("dx = %.9f\n", dx);
    printf("dx = %.19f\n", dx);
    printf("ldx = %.19Lf\n", ldx);
    return 0;
}
```

Original Source: [http://woogyun.tistory.com/391](http://woogyun.tistory.com/391)
