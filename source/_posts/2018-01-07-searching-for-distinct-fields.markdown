---
layout: post
title: "MySQL - Searching for distinct fields"
date: 2018-01-07 21:42:23 -0200
comments: true
categories: [mysql]
---

Quando não se há certeza onde determinada(s) coluna(s) a qual se deseja buscar se encontra,
podemos pesquisar por todas tabelas do banco, utilizando a <!--more--> linha de comando MySQL, veja um exemplo:

```sql
SELECT DISTINCT TABLE_NAME 
    FROM INFORMATION_SCHEMA.COLUMNS
    WHERE COLUMN_NAME IN ('coluna_pesquisar')
AND TABLE_SCHEMA='no_database';
```
Onde coluna_pesquisar é a coluna que deseja pesquisar e no_database, é o database que buscaremos a coluna.
