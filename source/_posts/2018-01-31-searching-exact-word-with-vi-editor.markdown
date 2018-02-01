---
layout: post
title: "Searching exact word with vi editor"
date: 2018-01-31 15:41:24 -0200
comments: true
categories: linux
---

Para pesquisar uma palavra exata no VI(M), basta colocar a palavra entre as <!--more--> expressões
regulares \\< e \\>, por exemplo:

```bash
/\<FOO\>
```

Isso por exemplo, retornará somente a palavra **FOO**, em vez de **FOOBAR**, **BARFOO**, etc...
