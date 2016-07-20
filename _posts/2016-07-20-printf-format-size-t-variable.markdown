---
layout: post
title:  "printf format size_t variable"
date:   2016-07-20 10:20:00 +0900
categories: util
---

Use the z modifier(C99 addition):

```
size_t x = ...;
ssize_t y = ...;
printf("%zu\n", x);  // prints as unsigned decimal
printf("%zx\n", x);  // prints as hex
printf("%zd\n", y);  // prints as signed decimal
```

Looks like it varies depending on what compiler you're using (blech):

* gnu says `%zu` (or `%zx`, or `%zd` but that displays it as though it were signed, etc.)
* Microsoft says %Iu (or `%Ix`, or `%Id` but again that's `igned`, etc.)
