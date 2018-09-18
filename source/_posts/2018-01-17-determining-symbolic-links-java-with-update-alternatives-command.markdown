---
layout: post
title: "Determining symbolic links Java with update-alternatives command"
date: 2018-01-17 16:01:01 -0200
comments: true
categories: [java,linux]
---

With the utility **update-alternatives**, is possible you set versions of the Java installed in your Operating System Linux(which supports it),
without need add or <!--more--> remove symbolic link manually. To list one or more Java version available in target system, you can just typing the
command **update-alternatives**, adding the argument **\-\-list**, together with name of the symlink(Symbolic Link) desired. For example:

{% highlight bash %}
update-alternatives --list java
{% endhighlight %}

{% img rigth /images/output_update_alternative_list.jpeg 1800 1800 'output perl group' %}

Where **java** is the name of the symbolic link.

Also it is possible, you to install a new version, if by chance it hasn't on the list of the **update-alternatives**. You can thus use the parameter
**\-\-install**, setting the following arguments:

<ol>
	<li> The value properly of the desired symbolic link name. </li>
	<li> The full path of the binary Java. </li>
	<li> Lastly, specifying the level of priority associated with it. </li>
</ol>


For example:

{% highlight bash %}
update-alternatives --install /usr/bin/java java /opt/jdk1.8/bin/java 1
{% endhighlight %}

After typing the command above, you can use the parameter **\-\-list** to confirming the update:

{% highlight bash %}
update-alternatives --list java
{% endhighlight %}

To change to a new version, execute **update-alternatives** with parameter **\-\-config**, it will show a menu to you choice your option through
interactive way form:

{% highlight bash %}
update-alternatives --config java
{% endhighlight %}

{% img rigth /images/output_update_alternatives_config.jpeg 1800 1800 'output perl group' %}

There is another non-interactive way to you choice the version, just setting with parameter **\-\-set** or **-s**, instead of **\-\-config**, as below:

{% highlight bash %}
sudo update-alternatives --set java /opt/jdk1.8/bin/java
{% endhighlight %}

For more info, enter with:

** *man update-alternatives* **

