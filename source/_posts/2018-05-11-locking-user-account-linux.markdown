---
layout: post
title: "Locking user account Linux"
date: 2018-05-11 12:48:04 -0300
comments: true
categories: linux
---

The following command will lock an user <!--more--> account:

{% highlight bash %}
passwd --lock foo
{% endhighlight %}

or simply

{% highlight bash %}
passwd -l foo
{% endhighlight %}

Remember that, you should run the above command as root account.

If you want unlocking the account foo, just type the **passwd** command with **-u** or **--unlock** parameter, e.g:

{% highlight bash %}
passwd --unlock foo
{% endhighlight %}

or simply

{% highlight bash %}
passwd -u foo
{% endhighlight %}
