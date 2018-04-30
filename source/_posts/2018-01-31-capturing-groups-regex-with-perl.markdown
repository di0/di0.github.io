---
layout: post
title: "Capturing groups regex with Perl"
date: 2018-01-31 20:46:12 -0200
comments: true
categories: perl
---

Capturing groups in regular expression, against a scalar type, it's possible when you use the variable <!--more--> number, such as $1, $2, $3..., for example:

```perl

#!/usr/bin/env perl

print "Enter your name and lastname here => ";
chop ( $name_and_lastname = <STDIN> );

if ( $name_and_lastname =~ /^\s*(\S+)\s+(\S+)\s*$/ )
{
	print "Hello $1. Your lastname: $2.";
}
else
{
	print "no result";
}
print "\n";

```

** output: **

{% img rigth /images/output_perl_group.jpeg 1800 1800 'output perl group' %}

