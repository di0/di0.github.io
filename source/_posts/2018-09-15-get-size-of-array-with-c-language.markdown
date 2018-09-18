---
layout: post
title: "Get size of array with C language"
date: 2018-09-15 03:20:58 -0300
comments: true
categories: c/c++
---

Language C has not function available that recover size of array by default. However, we
can use a trick<br>that do possible get such size. Look like on example below:<!--more-->

```c
#include <stdio.h>

int
main()
{
        int a[ 99 ];
        size_t n = sizeof( a ) / sizeof( int );
        printf( "%d\n", n );
}
```

but, if you chance the type of array, you can have problems because the expression above,
use int type. To avoid this, would be used this way:

```c
#include <stdio.h>

int
main()
{
        int a[ 99 ];
        size_t n = sizeof( a ) / sizeof( a[ 0 ] ); // Here, we get the type of array
        printf( "%d\n", n );
}
```

In example abouve, we get the first field of the array, what is sufficient to calculate
the type of array.
