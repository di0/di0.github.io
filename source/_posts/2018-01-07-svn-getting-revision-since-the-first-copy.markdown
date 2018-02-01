---
layout: post
title: "SVN - Getting revision since the first copy"
date: 2016-11-01 23:27:11 -0200
comments: true
categories: [svn]
---

Para obter o primeiro commit de onde foi <!--more--> criada a branch:

```bash
svn log --stop-on-copy
```
