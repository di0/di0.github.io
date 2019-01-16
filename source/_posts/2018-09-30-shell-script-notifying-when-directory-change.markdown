---
layout: post
title: "Shell Script - Notifying when directory change"
date: 2018-09-30 04:23:46 -0300
comments: true
categories: shell_script
---

One of these days, I was needed to monitor a directory that constantly was modified. I had needed some way, get
something that could notify me when the structure(file system) from directory <!--more--> was modified. So, I
decided to write my own script, whose helped me with this task.

For it, was used the Linux **stat** utility, whose display the detailed status of a particular file or a file
system.

Below, is show you how it works:

{% highlight bash %}
#!/bin/bash

LTIME=`stat -c %Z .`

while true
do
        ATIME=`stat -c %Z .`

        if [[ "$ATIME" != "$LTIME" ]]
        then
                full_name=`/bin/ls | head -1`
                name=$(basename "$full_name")
                extension="${name##*.}"
                final_name="${name%.*}"

                if [ "$extension" == "txt" ]
                then
                        if [ ! -f $final_name.log ]
                        then
                                touch $final_name.log
                                echo "Modified ATIME" >> $final_name.log
                        fi
                fi
                LTIME=$ATIME
        fi
        sleep 5
done
{% endhighlight %}

So, the code above consists a script that checks if, a new text file was created in current directory.
If it has been, then, it verifies if already exists a file name called **final_name.log**, and case
it not exists, it will be created.

In begin, the script get the initial current time across stat command and holds it inside variable
LTIME. It variable later, will be used to compare with 

{% highlight bash %}
LTIME=`stat -c %Z .`
{% endhighlight %}

