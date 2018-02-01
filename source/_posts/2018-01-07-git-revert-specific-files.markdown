---
layout: post
title: "Git - Revert specific files"
date: 2017-10-20 22:12:50 -0200
comments: true
categories: [git]
---

Assumindo hipoteticamente que o hash para qual deseja reverter um ou mais arquivos é **cf762e4c187b**.

O comando abaixo irá reverter os arquivos foo1.txt e foo2.txt para o <!--more--> hash informado:

```bash
git checkout cf762e4c187b -- dir/foo1.txt dir/foo2.txt
```
