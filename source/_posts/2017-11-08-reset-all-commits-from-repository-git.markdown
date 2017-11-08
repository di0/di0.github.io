---
layout: post
title: "Reset all commits from Repository Git"
date: 2015-09-24 00:26:40 -0200
comments: true
categories: [Git]
---

Para remover todos os históricos de commits e iniciar novamente o commit inicial, são dados os seguintes passos:

1) Remover o .git do repositorio(local) que deseja resetar</br>
2) Recriar o artefato do banco de dados git, seguindo os passos abaixo:

``` bash
$ cd Project
$ git init
// crie alguns arquivos ( eu clonei do SVN, em vez disso )
$ git add .
$ git commit -m ‘Initial commit’
```

3) Realizar o push para o servidor remoto**(troque a url pela url do repositorio alvo)**, forçando a sobreescrita:

``` bash
$ git remote add origin <url>
$ git push ‐‐force ‐‐set-upstream origin master
```
