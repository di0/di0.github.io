---
layout: post
title: "Splitting a subdirectory into a new repository"
date: 2019-02-28 15:21:58 -0300
comments: true
categories: git
---

First you must to clone the repository target: <!--more-->

{% highlight bash %}
git clone https://lognull.com/PocProjectX.git
{% endhighlight %}

Filter the directory that will be send to another repository

{% highlight bash %}
git filter-branch --prune-empty --subdirectory-filter src/main/java/com/develdio/poc/robot/remote/ develop
{% endhighlight %}

Add the new URL in current repository

{% highlight bash %}
git remote set-url origin https://develdio.com/PocProjectXOnlyRemote.git
{% endhighlight %}

Update remote refs along with associated objects

{% highlight bash %}
git push origin develop
{% endhighlight %}

Undo the current changes and returns the original state:

{% highlight bash %}
git reset --hard refs/original/refs/heads/develop
{% endhighlight %}
