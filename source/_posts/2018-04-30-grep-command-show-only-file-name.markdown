---
layout: post
title: "show only file name with grep command"
date: 2018-04-30 00:48:26 -0300
comments: true
categories: linux
---

With **grep** and **cut** delimiter command we can get only file name where the occurrence of the input match. Below an<!--more--> 
example:

```bash
grep -Rn "foo" * | cut -f1 -d:
```

**output**

{% img rigth /images/output_grep_cut.jpeg 1800 1800 'output grep' %}
