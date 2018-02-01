---
layout: post
title: "git status with grep"
date: 2015-05-15 03:36:05 -0200
comments: true
categories: [git,linux]
---

Para manter as cores na saída padrão, ao usar o comando **git status** junto aos
utilitários **grep**, **less** ou **more**, <!--more--> basta adicionar a opção **-c color-status=always** ao
executar o comando **git status**. Por exemplo:

``` bash
git -c color.status=always status | grep -i "alguma coisa"
```

Observe na sintaxe do comando, o argumento **-c color.status=always** vem antes do argumento **status**.
Do contrário, um erro ocorrerá.

Para os comandos **git diff**, **git show** ou **git log**, a sintaxe é a mesma, exceto que, a
variável utilizada dessa vez é a **color.ui**, como exemplo abaixo:

``` bash
git -c color.ui=always log | grep -i -B 2 -A 3 "alguma coisa"
```
