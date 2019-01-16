---
layout: post
title: "Reading parameters from function - C"
date: 2018-09-21 23:00:41 -0300
comments: true
categories: c/c++
---

In this post, I was write as we can get the parameters given to a function in language C, using
stdarg library, that declares a type va_list and defines three macros for<!--more--> stepping
through  a list of arguments whose number and types are not known to the called function.

Below is show the declared functions in stdarg:

{% highlight c %}
#include <stdarg.h>

void va_start( va_list ap, last );
type va_arg( va_list ap, type );
void va_end( va_list ap );
void va_copy( va_list dest, va_list src );
{% endhighlight %}



```c
#include <stdio.h>
#include <stdarg.h>

void read_names( char *name_main, ... )
{
        va_list args;
        va_start( args, name_main );

        char *name1 = va_arg( args, char * );
        char *name2 = va_arg( args, char * );

        fprintf( stdout, "%s => %s and %s\n", name_main, name1, name2 );
        va_end( args );
}

int
main()
{
        read_names( "Main name", "second name", "third name" );
}
```

As shown above, we can see that use the function **va_start**, which is declared in stdarg.h header. This
function is responsible to get a varying number of arguments of varying types.

The **var_start** macro, initializes **va_list** for subsequent use by **va_arg()** and **va_end()**, and must be called
first. 

