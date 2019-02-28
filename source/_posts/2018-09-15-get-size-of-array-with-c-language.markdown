---
layout: post
title: "Get size of array with C language"
date: 2018-09-15 03:20:58 -0300
comments: true
categories: c/c++
---

Language C has not function available that recover size of array by default. However, we
can use a trick that do possible get such size. Look like on example below:<!--more-->

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

All right, the output will be **99**. Thus, if you change the array to char type, for example, you can have
problems because the expression above use **int** type.

As see above, we know that, the C language use the unary operator function **sizeof**, that compute the size of
type(char, int, float, double...) in compile time. So, the **int** type has a different size of **char** type.
Also, the result of expression can be different relative from machine architecture. eg: 32 bits or 64 bits.

Then, to avoid this, we should use something like this way:

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

In example above, we get the first field of the array(***sizeof(a[0])***), instead to compute by type(***sizeof(int)***). It was
sufficient to compute array size, independent of data type her. Notice also that, this became more generic expression.
