---
layout: post
title: "Pointer to function with C/C++"
date: 2018-09-15 03:14:27 -0300
comments: true
categories: c/c++
---

Like any entity stored in memory, a function also has your own address and other resources stored on dynamic memory
of operation system.<!--more-->
Therefore, is possible have a pointer variable that pointer to the address to function stored on memory. The syntax to
a pointer to function basically resembles this:

<span style="color:teal;font-weight:bold">type_return (*variable_name)( type data of one and/or more parameters or no parameter )</span>

For example:

{% highlight c %}
int ( *pointer_f )( int );
{% endhighlight %}

In above example, we create a pointer that points to a function that has a type integer as parameter and return a type
integer. Notice that, we use parentheses in the function declaration to alter precedence, which is greater than the asterisk(*), otherwise, without
this, the compiler interprets the declaration as a **function** that will return an integer type and get an integer as parameter, look how:

{% highlight c %}
int *pointer_f( int );
{% endhighlight %}

As previously described, we can use this pointer to a function that has an integer type parameter and also returns an integer type.
See like it works:

{% highlight c %}
int calculate_ip_broadcast( char * subnet )
{
	unsigned int bits = atoi( remove_dots( subnet ) ) ^ 0xffffffff;
	return bits;
}

pointer_f = calculate_ip_broadcast;
{% endhighlight %}

Notice that **pointer_f** now, points to the address from **calculate_ip_broadcast** function. Once pointed to target function, we can
invoke the function through pointer, like any another function invocation:

{% highlight c %}
int r = pointer_f( "172.16.16.0" );
{% endhighlight %}

or, if we prefer, we may:

{% highlight c %}
int r = ( *pointer_f )( "172.16.16.0" );
{% endhighlight %}

With pointer function, we can work with **call back** techniques, allow us perform tasks complex of form flexible. 

<h3 style="color:navy;">More about function pointer</h3>

Now that we known how works function pointer, let's go learn more about this. 

In C language we can work with function inside another as parameter, hypothetically consider the following code sample:

{% highlight c %}
int * on_calculate( int f_calculate( int value ) )
{
	// ... something
	int result = f_calculate( 0x00 );
	int *p_result = &result;
	return p_result;
}

int calculate_form_one( int value )
{
	int exp = value / 0xff;
	return exp;
}

int calculate_form_two( int value )
{
	int exp = ( value / 0xff ) * 0xff;
	return exp;
}

void
main( void )
{
	/*
	 * Let just passing function reference name, whose will invoke later.
	 */

	// this way:

	int *result = on_calculate( calculate_form_one );
	fprint( stdout, "Result: %d\n", *result );

	// or

	result = on_calculate( calculate_form_two );
	fprint( stdout, "Result: %d\n", *result );
	
}
{% endhighlight %}

We should not worry on details, just focus how it is possible and how we work with function inside another function as parameter.

Therefore, the following code snippet below, shows us a pointer to a function that get an another function
pointer as parameter:

{% highlight c %}
int * ( *pointer_f )( int ( *inside_pointer_f )( int ) );
{% endhighlight %}

Okay, that is really very obscure... this should to be explained:

**pointer_f** is the  pointer variable name that receive another function pointer called **inside_pointer_f** as parameter that
receives an int and return int. The **pointer_f** pointer then, finally return a pointer to integer type.

So, with the pointer **pointer_f** quoted previously, we are able to point to the function like it:

{% highlight c %}
int * on_calculate( int calculate( int value ) );
{% endhighlight %}

Look as this is possible:

{% highlight c %}
int * ( *pointer_f )( int ( *inside_pointer_f )( int ) );
pointer_f = on_calculate;
{% endhighlight %}

Then, we invoke this way:

{% highlight c %}
printf( "%d\n", *pointer_f( some_calculate_function ) );
{% endhighlight %}

Above, we pass the function responsible by calculate, through function pointer. Notice that this becomes so much
flexible, we need just give calculate responsible function, then, we call back as required:

{% highlight c %}
printf( "%d\n", *pointer_f( calculate_form_one ) );
printf( "%d\n", *pointer_f( calculate_form_two ) );
// and so on...
{% endhighlight %}

The declaration **int * ( *pointer_f )( int ( *inside_pointer_f )( int ) )** can be often times a confuse syntax and declare this every
time, is really so hard. Because of this, we can use the **typedef** reserved keyword, to define a type to hidden this complexity.

{% highlight c %}
typedef int * ( *pointer_f )( int ( *inside_pointer_f )( int ) );
{% endhighlight %}

Now we can use this form:

{% highlight c %}
pointer_f * pointer_function;
pointer_function = on_calculate;
printf( "%d\n", *pointer_function( calculate_form_one ) );
printf( "%d\n", *pointer_function( calculate_form_two ) );
{% endhighlight %}

Below, follows a complete example:

{% highlight c %}
#include <stdio.h>

/*
 * Function pointer that returns a pointer to integer type. This function has
 * three parameters. One of them is a pointer to function that get two integers
 * parameters and that return an integer type. Anothers two parameters are an
 * integer types.
 */
typedef int * ( *pointer_f )( int ( *inside_pointer_f )( int, int ), int, int );

int *
on_calculate( int calculate( int, int ), int valueX, int valueY )
{
	int result = calculate( valueX, valueY );
	int *p_result = &result;
	return p_result;
}

int
multiplication( int valueX, int valueY )
{
	return valueX * valueY;
}

int
addition( int valueX, int valueY )
{
	return valueX + valueY;
}

int
subtraction( int valueX, int valueY )
{
	return valueX - valueY;
}

int
division( int valueX, int valueY )
{
	return valueX / valueY;
}

int
main( void )
{
	// References the function.
	pointer_f pointer_function = on_calculate;

	// Now we can to dereferences the function and gives it another
	// function as parameter that we desired to calculate.

	printf( "%d\n", *pointer_function( multiplication, 5, 5 ) );
	printf( "%d\n", *pointer_function( addition, 10, 7 ) );
	printf( "%d\n", *pointer_function( subtraction, 5, 2 ) );
	printf( "%d\n", *pointer_function( division, 15, 3 ) );
}
{% endhighlight %}

And, the expected output respectively are:

{% highlight bash %}
25
17
3
5
{% endhighlight %}

<h3 style="color:navy;">Returning pointer to function</h3>

It is possible also, returns pointer to function, according syntax above:

{% highlight c %}
int( *return_point_to_function() )()
{
	int ( *pnt_fn_ret_integer_type )() = some_function_that_return_int;
	return pnt_fn_ret_integer_type;
}
{% endhighlight %}

Is important emphasizes that, rather than we use "int **( *return_point_to_function )()**", we use the
**parentheses** after name **return_point_to_function**, because else, the compiler could to intepret
it as a declaration of pointer to a function and not a function that returns a pointer to function.

Let's go see a practice example. Let us suppose that we have this functions:

{% highlight c %}
int
get_number_of_users_actives()
{
	return n_users_actives;
}

int
get_number_of_users_inactives()
{
	return n_users_inactives;
}
{% endhighlight %}

With our function that returns a pointer to function previously quoted, we could do something like it:

{% highlight c %}
int
( *get_number_of_users() )()
{
	int ( *pointer_function )() = get_number_of_users_actives;
	// or
	int ( *pointer_function )() = get_number_of_users_inactives

	return pointer_function;
}
{% endhighlight %}

As we can see, the function above returns a pointer to one of two function desired.

To show this working, follows an example more detail what we can do:

{% highlight c %}
#include <stdio.h>

#define ACTIVE 1
#define INACTIVE 0

int
get_number_of_users_actives()
{
	int n_users_actives = 10;
	return n_users_actives;
}

int
get_number_of_users_inactives()
{
	int n_users_inactives = 50;
	return n_users_inactives;
}

int
( *get_number_of_users( int is_active ) )()
{
	int (*pointer_function)();
	if ( is_active )
		pointer_function = get_number_of_users_actives;
	else
		pointer_function = get_number_of_users_inactives;

	return pointer_function;
}

int
main()
{
	int ( *result )() = get_number_of_users( ACTIVE );
	printf( "Users actives: %d\n", result() );

	result = get_number_of_users( INACTIVE );
	printf( "Users inactives: %d\n", result() );
}
{% endhighlight %}

The output is:

{% highlight bash %}
Users actives: 10
Users inactives: 50
{% endhighlight %}

Again, is advisable to use the **typedef keyword** to facilitate the use this complex syntax and becomes it
more readability. For instance, see how would be with typedef:

{% highlight c %}
typedef int ( *get_number_of_user )();
typedef get_number_of_user POINT_TO_GET_NUMBER_USER;
{% endhighlight %}

So, we can change our function **get_number_of_users** to this:

{% highlight c %}
POINT_TO_GET_NUMBER_USER get_number_of_users( int is_active )
{
	POINT_TO_GET_NUMBER_USER p_get_number;
	if ( is_active )
		p_get_number = get_number_of_users_actives;
	else
		p_get_number = get_number_of_users_inactives;

	return p_get_number;
}
{% endhighlight %}

and finally, in main function, would be:

{% highlight c %}
int
main()
{
	POINT_TO_GET_NUMBER_USER result = get_user( ACTIVE );
	printf( "Users actives: %d\n", result() );

	result = get_user( INACTIVE );
	printf( "Users inactives: %d\n", result() );
}
{% endhighlight %}

<p style="border:3px; border-style:solid; border-color:#FF0000; padding: 1em;"><strong>Important!</strong><br><br>

When we use typedef keyword to define a function pointer, we shouldn't use the parentheses as we had done previously
in function pointer definition. Look:
<br>
<br>
<strong>int * ( *point_f )();</strong><br>
Above, was declared a function pointer that return a type integer pointer.
<br>
<br>
<strong>int * ( *point_f() )() { ... }</strong><br>
Above, was implemented a function called point_f that return a function pointer that return a type integer pointer.
<br>
<br>
<strong>typedef int * ( *point_f )();</strong><br>
Above, was declared with typedef keyword a function pointer that return a type integer pointer.
<br>
<br>
Notice that, with typedef keyword, we do not use the parentheses after name, it because we do it when we create a function
with using the typedef definition:<br><br>

<strong>point_f</strong> do_something()<br>
{<br>
....<br>
}<br><br>

If we use parentheses in typedef keyword, we will get errors in compilation time.
</p>

Now look this curious case:

{% highlight c %}
int
addition( int valueX, int valueY )
{
        return valueX + valueY;
}

int
( *calculator() )()
{
	int (*p_function)( int, int ) = addition;
        return p_function;
}
{% endhighlight %}

Notice that, the **calculator function**, returns a function pointer that receives two integers types and returns a
type integer. However, the **signature** from function tell us this:

{% highlight c %}
int ( *calculator() )();
{% endhighlight %}

This occurred because the compiler C, infers the parameters type from pointer function and **allow** us omits it, otherwise, we
should do it this way:

{% highlight c %}
int ( *calculator() )( int, int );
{% endhighlight %}

If by change we specific the parameters types in **calculator** function, we are obliged to put the two parameters and not just one:

{% highlight c %}
int ( *calculator() )( int ); // Error compile
{% endhighlight %}

The same occurs in declaration:

{% highlight c %}
int
main()
{
	// We can use this way
	int ( *result )() = calculator();

	// Or this way
	int ( *result )( int, int ) = calculator();

	// But we cannot this way
	int ( *result )( int ) = calculator();
}
{% endhighlight %}

Finally, the last example of how we can use function pointer. Here we show a function pointer that
returns a pointer to array of function of the type integer.

{% highlight c %}
int ( **pointer_function_to_array_of_functions_int() )();
{% endhighlight %}

As show above, we have a function that returns a function pointer to pointer, that points to an array
of function that returns an integer type. With this function, we can returns something like this:

{% highlight c %}
int ( *function_pointer_int[] )() = { function_int_one, function_int_two, ... };
{% endhighlight %}

All right, this is more easier with an example. Let rewrites our code used previously that calculate two
numbers with four operations mathematic:

{% highlight c %}
#include <stdio.h>
#include <stdlib.h>

typedef int * ( *FUNCT_PNT )( int, int );

int
multiplication( int valueX, int valueY )
{
	return valueX * valueY;
}

int
addition( int valueX, int valueY )
{
	return valueX + valueY;
}

int
subtraction( int valueX, int valueY )
{
	return valueX - valueY;
}

int
division( int valueX, int valueY )
{
	return valueX / valueY;
}

int ( *p_list_fn_operations[] )() = { multiplication, addition, subtraction, division };

int
( **calculator() )( int, int )
{
	/**
	 * There's no need to use an address-of operator on array, like this:
	 * int ( **p_fn_operations )( int, int ) = &p_list_fn_operations[ 0 ];
	 * since arrays decay into implicit pointers on the right-hand side of
	 * the assignment operator.
	 */
	int ( **p_fn_operations )( int, int ) = p_list_fn_operations;
	return p_fn_operations;
}

int
main()
{	
	// Arrays are implicitly convertible to pointers, so it would be
	// the syntax below with double pointer:
	int ( ** ( *pointer_function_to_calculator )() )();
	pointer_function_to_calculator = calculator;
	int ( **list_of_operation )();

	list_of_operation = ( *pointer_function_to_calculator )();

	// For each operations, we use an increment arithmetic to go through
	// of array of pointer, searching by each of that operations.
	printf( "Multiplication = %d\n", ( * list_of_operation )( 6, 6 ) );
	list_of_operation++;

	printf( "Addition = %d\n", ( * list_of_operation )( 10, 7 ) );
	list_of_operation++;

	printf( "Subtraction = %d\n", ( * list_of_operation )( 10, 4 ) );
	list_of_operation++;

	printf( "Division = %d\n", ( * list_of_operation )( 81, 9 ) );
}
{% endhighlight %}

The output will be:

{% highlight bash %}
Multiplication = 36
Addition = 17
Subtraction = 6
Division = 9
{% endhighlight %}

<h3 style="color:navy;">Extra about function pointers</h3>

In C language, the library signal.h have a function called **signal**, that provides a simple interface for
establishing an action for a particular signal.

The said function is the **sighandler_t** function, whose is a function pointer that points to a function
that returns void. That function will go handlers the integer type argument givens, configuring your
respective signal number. So, functions like this, are handles this way:

{% highlight c %}
void handler (int signum) { … }
{% endhighlight %}

And here, the signal function itself:

**sighandler_t signal( int signum, sighandler_t action )**

The signal function establishes action as the action for the signal signum.

The first argument, **signum**, identifies the signal whose behavior we want to control, and should be a signal number
The second argument, **action**, specifies the action( function ) that will be handlers the signal.

{% highlight bash %}
#include <signal.h>
typedef void (*sighandler_t)(int);
sighandler_t signal(int signum, sighandler_t handler);
{% endhighlight %}

For more details, type man signal.

Another famous utility in C language is the **qsort** function, from **stdlib.h** library, that have the following declaration:

{% highlight c %}
void qsort( void *base, size_t nmemb, size_t size,
            int( *compar )( const void *, const void * ));
{% endhighlight %}

The Linux manual page, describe the following:


*The qsort() function sorts an array with nmemb elements of size size. The base argument points to the start of the array.*

*The contents of the array are sorted in ascending order according to a comparison function pointed to by compar, which is*
*called with two arguments that point to the objects being compared.*

*The comparison function must return an integer less than, equal to, or greater than zero if the first argument is considered*
*to be respectively less than, equal to, or greater than the second. If two members compare as equal, their order in the*
*sorted array is undefined.*

So, here we going see an example of how this works:

{% highlight c %}
#include <stdio.h>
#include <stdlib.h>

int desc_sort( const void *v1, const void *v2 )
{
	int first = *( int *) v1;
	int second = *( int * ) v2;

	if ( first > second )
		return -1;
	else if ( first == second )
		return 0;
	else
		return 1;
}

int main()
{
	int array[ 10 ];

	int i;
	for ( i = 0; i < 10; ++i )
		array[ i ] = i;

	qsort( array, 10 , sizeof( int ), desc_sort );

	for ( i = 0; i < 10; ++i )
		printf ( "%d\n", array[ i ] );
}
{% endhighlight %}

the output will be:

{% highlight bash %}
9
8
7
6
5
4
3
2
1
{% endhighlight %}

For more details, type man qsort.
