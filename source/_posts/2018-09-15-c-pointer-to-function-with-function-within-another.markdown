---
layout: post
title: "C - Pointer to function with function within another function"
date: 2018-09-15 03:14:27 -0300
comments: true
categories: c/c++
---

In this example, you can see as we can use pointer to a function that taken an inner
function as parameter. <!--more-->


```c
#include <stdio.h>

int *
function1( int g( int c ) )
{
        int r = g( 14 );
        int *p = &r;
        return p;
}

int
function2( int c )
{
        return c + 1;
}

int
main( void )
{
        // Pointer to function that return pointer to int type and that
        // taken as parameter a pointer to function that return a int and
        // have as parameter an int type.
        int * ( *p )( int (*pp)( int ) );

        // Reference the function.
        p = function1;

        // Desreference the function and reference the function on parameter.
        // Within function that p point, will be invoke the function function2.
        printf( "%d\n", *p( function2 ) );
}
```
