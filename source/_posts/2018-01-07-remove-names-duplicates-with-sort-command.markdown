---
layout: post
title: "Remove names duplicates with sort command"
date: 2016-05-20 20:37:51 -0200
comments: true
categories: [linux]
---

É possível remover nomes duplicados de uma saída padrão com o utilitário **sort**. Exemplos:

```bash
sort -u foo.txt
```
ou

```bash
sort file.txt | uniq

```

É possível também, imprimir somente os valores repetidos, em vez de ocultá-los, segue:

```bash
sort file.txt | uniq -d
```

Uma outra forma de realizar a mesma tarefa que faz o comando **sort** é a de usar o comando
**awk**, conforme exemplo abaixo:

```bash
$ awk '!a[$0]++' foo.txt
```
