---
layout: post
title: "Ignore temporarily files with git"
date: 2012-05-25 04:46:06 -0200
comments: true
categories: [git]
---

A maneira de ignorar arquivos nos commits do git é através do arquivo de configuração chamado
**gitignore**. Basta adicionar nele arquivos ou diretórios que não serão colocados na área de commit
na geração de um novo snapshot.

Há uma outra maneira de ignorar temporariamente arquivos ou diretórios de maneira dinâmica, em
vez de adicionar de maneira estática, como acontece ao utilizar o gitignore. A maneira de se
fazer isso, é através do argumento **update-index --assume-unchanged**, de tal forma:

```bash
git update-index --assume-unchanged foo.txt
```

Isso fará com que o arquivo não apareça mais em comandos como **git status**, **git diff**...

Por consequência, não será colocado na área de stage para o próximo commit.

Para retornar o arquivo a ser monitorado, basta desfazer a método de ignore, utilizando
dessa vez o argumento **update-index --no-assume-unchanged**, da seguinte forma:

```bash
git update-index --no-assume-unchanged foo.txt
```
