---
layout: post
title: "Changing a commit message in SVN"
date: 2014-05-15 02:50:04 -0200
comments: true
categories: svn
---

Desde que esteja habilitado pelo administrador do servidor SVN, há ao menos duas maneiras de modificar as mensagens previamentes comitadas no SVN:

A primeira modifica a mensagem remotamente, com uma das duas formas:

``` bash
$ svn propedit -r N --revprop svn:log URL
$ svn propset -r N --revprop svn:log "nova mensagem" [URL do Repositório]
```

Onde **N** é o número da revisão

A segunda apenas modifica a mensagem localmente através do utilitário svnadmin:

``` bash
$ svnadmin setlog [PATH do Repositório] -r N Arquivo
```

Onde **N** é o número da revisão
