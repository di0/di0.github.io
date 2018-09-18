---
layout: post
title: "Creating and Saving Stash List With Git"
date: 2018-09-15 02:33:18 -0300
comments: true
categories: git
---

When stash the changes in a dirty working directory away, we can can give a more descriptive<br>
message on the command line when we create one. By <!--more-->example:


{% highlight bash %}
git stash save "Saving anything"
{% endhighlight %}

The above order, will push and holds on stack, the contents with descriptive message.

{% highlight bash %}
git stash list
{% endhighlight %}

The command above, will show you, what is the branch name and what position that contents
was saved on stack.

So, to pick up first contents from stack, type:

{% highlight bash %}
git stash pop stash@{0}
{% endhighlight %}

or second

{% highlight bash %}
git stash pop stash@{1}
{% endhighlight %}

and son on ...
