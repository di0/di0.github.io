---
layout: post
title: "Flush Contents of a Memcached Server"
date: 2018-04-25 22:12:26 -0300
comments: true
categories: linux
---

The two ways following examples, show us how flush old data from memcached server through
command line. We use netcat and <!--more--> telnet command to perform this job.

<h4><u>Using netcat Command</u></h4>

**netcat** command is a simple unix utility which reads and writes data across network connections, using
TCP or UDP protocol. Bellow you can see how such commands works:

{% highlight bash %}
echo 'flush_all' | nc localhost 11211
or
echo 'flush_all' | netcat localhost 11211
{% endhighlight %}

*another form way:*

{% highlight bash %}
nc 192.168.1.10 11211<<<"flush_all"
{% endhighlight %}

*with alias setting:*

{% highlight bash %}
alias flush_mem_cache_server="echo 'flush_all' | netcat 127.0.0.1 11211"
{% endhighlight %}

Now, just you run the alias **flush_mem_cache_server** in your bash shell.

<h4><u>Using telnet Command</u></h4>

We can as well use the telnet protocol via default port(11211) from memcached server, typing **flush_all**. For example:

{% highlight bash %}
telnet 192.168.1.10 11211
{% endhighlight %}

then output will looks like:

{% img rigth /images/output_telnet_memcached.jpeg 1800 1800 'output perl group' %}
