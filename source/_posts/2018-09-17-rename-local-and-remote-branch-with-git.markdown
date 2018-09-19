---
layout: post
title: "Rename local and remote branch with git"
date: 2018-09-17 15:47:55 -0300
comments: true
categories: git
---

If you want rename a branch local and remote, follow these steps:<!--more-->

First, rename the target branch:

{% highlight bash %}
git branch -m new-name
{% endhighlight %}

Or, if by chance, you desire rename another branch, whose is not current branch, do it such as:

{% highlight bash %}
git branch -m old-branch-name new-branch-name
{% endhighlight %}

Second, remove remote branch and push the new local branch:

{% highlight bash %}
git push origin :old-branch-name new-branch-name
{% endhighlight %}

And finally, load the upstream branch for the new local branch:

{% highlight bash %}
git checkout new-branch-name
git push origin -u new-branch-name
{% endhighlight %}



