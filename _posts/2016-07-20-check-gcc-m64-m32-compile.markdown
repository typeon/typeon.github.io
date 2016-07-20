---
layout: post
title:  "check gcc compile -m64, -m32"
date:   2016-07-20 10:40:00 +0900
categories: util
---

sample code:

{% highlight c++ %}
#include <stdio.h>

void main()
{
    unsigned long p;

    printf("%p\n", &p);

// 64bit compile check. -m64, -m32
//
#if __LP64__
    printf("__LP64__ defined...\n");
#else
    printf("__LP64__ not defined...\n");
#endif

    printf("p's sizeof is %zu\n", sizeof(p));
}
{% endhighlight %}

*Note: gcc only.*

output:

```
$ gcc -o bitmode_compile bitmode_compile.c -m64 && ./bitmode_compile
0x7ffe5fe59380
__LP64__ defined...
p's sizeof is 8

$ gcc -o bitmode_compile bitmode_compile.c -m32 && ./bitmode_compile
0xffbf1fb8
__LP64__ not defined...
p's sizeof is 4
```

original source: [http://kaspyx.kr/78](http://kaspyx.kr/78)
