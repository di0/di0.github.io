---
layout: post
title: "Compress file with git diff output"
date: 2014-04-24 04:00:14 -0200
comments: true
categories: [git,linux]
---

O comando abaixo demonstra como compactar um arquivo no formato tar, com arquivos 
que foram modificados são exibidos na saída comando git diff. Por exemplo:


```bash
git diff --name-only | xargs tar czf new-files.tar.gz
```

Isso criará um arquivo compactado com um ou mais arquivo retornado pelo **git diff --name-only**
