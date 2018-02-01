---
layout: post
title: "Capturing groups regex with Perl"
date: 2018-01-31 20:46:12 -0200
comments: true
categories: perl
---

Para obter grupos na comparação regex contra um tipo escalar, é possível utilizando
o <!--more--> número de variáveis( $1, $2, $3...) casadas nessa comparação:

```perl

#!/usr/bin/env perl

print "Enter your name and lastname here";
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
