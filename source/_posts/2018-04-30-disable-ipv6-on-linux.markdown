---
layout: post
title: "Disable IPv6 on Linux"
date: 2018-04-30 18:19:07 -0300
comments: true
categories: linux
---

**IPv6** protocol is not always available in the local area network(lan), to avoid your DHCP connection configure IPv6 in
your network card device, you can disable it through least <!--more--> two ways:

<h4><u>Command line</u></h4>

**sysctl** command is an Linux command that configures kernel parameters at runtime. You can display all
values currently available, typing **sysctl -a**. More details, see *sysctl manual page*.

Regarding configuration, with super admin user, type:

{% highlight bash %}
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w sysctl -w net.ipv6.conf.default.disable_ipv6=1
{% endhighlight %}

If you want re-enable IPv6, issue the following commands:

{% highlight bash %}
sysctl -w net.ipv6.conf.all.disable_ipv6=0
sysctl -w net.ipv6.conf.default.disable_ipv6=0
{% endhighlight %}

<h4><u>File configuration(Debian based distro)</u></h4>

Another way, is through of the file configuration located under directory **/etc/**.
The file name is **sysctl.conf**, which is a simple file containing sysctl values to be read in and set by **sysctl**.

With super admin user, edit the file /etc/sysctl.conf and add the following lines:

{% highlight bash %}
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
{% endhighlight %}

To disable, just remove the above lines.
