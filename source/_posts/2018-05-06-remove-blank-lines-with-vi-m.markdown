---
layout: post
title: "Remove blank lines with vi(m)"
date: 2018-04-06 16:37:27 -0300
comments: true
categories: linux
---
If you want delete blank lines using *VI(M)* editor, you can use the following match regex **^$** to perform it on the
command mode option(Esc + Shift + :) . e.g: <!--more-->

{% highlight bash %}
:g/^$/d
{% endhighlight %}


The above mentioned entry, will specify the *VI(M)* editor, delete blank lines.

<ul> g -> will execute command on all lines that match with regex.</ul>
<ul> d -> command order, delete that line.</ul>
