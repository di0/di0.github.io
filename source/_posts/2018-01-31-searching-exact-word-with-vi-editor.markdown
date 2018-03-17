---
layout: post
title: "Searching exact word with vi editor"
date: 2018-01-31 15:41:24 -0200
comments: true
categories: linux
---

Below I explain you how search an exact word in VI(M), what you just need to do, is to put there on between <!--more--> regular expression
\\< and \\>, for example:

```bash
/\<FOO\>
```

It will match only words whose name is **FOO**, rather than, **FOOBAR**, **BARFOO**, etc.
