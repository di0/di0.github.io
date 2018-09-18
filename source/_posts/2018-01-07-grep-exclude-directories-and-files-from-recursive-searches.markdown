---
layout: post
title: "Grep - Exclude directories and files from<br>recursive searches"
date: 2016-08-21 05:17:15 -0200
comments: true
categories: [linux]
---

Simplesmente, usando o parâmetro **\-\-exclude-dir** para excluir <!--more--> diretórios
e/ou **\-\-exclude** para excluir arquivos ou extensões de arquivos, tal como exemplo:

```bash
grep -r [Win] --color --exclude-dir=test --exclude-dir=target \
--exclude-dir=bin --exclude=\*.xml --exclude=\*.svn "pesquisa" .
```

Ou ainda:

```bash
$ grep -r [Win] --color --exclude-dir={test,target,bin} \
--exclude={*.xml,*.svn} "pesquisa" .
```

