---
layout: post
title: "Determining symbolic links Java with update-alternatives command"
date: 2018-01-17 16:01:01 -0200
comments: true
categories: [java]
---

Com o utilitário update-alternatives, é possível alternar entre versões do Java instaladas em um sistema Unix/Linux,
sem a necessidade de <!--more--> adicionar ou remover links simbólicos manualmente. Para listar uma ou mais versões do Java disponíveis
no sistema alvo, basta executar o comando update-alternatives, passando como argumento a opção **\-\-list**, junto ao nome do symlink(Link Simbólico) desejado, conforme exemplo abaixo:

```bash
update-alternatives --list java
```

Onde **java** é o nome do link simbólico.

É possível também, instalar uma nova versão, caso essa não esteja disponível na lista do update-alternatives. Para instalar uma nova versão do Java na lista, basta propriamente passar o argumento **\-\-install**, junto ao caminho onde o link estará disponível, mais o nome do symlink(Link Simbólico), mais o caminho da aplicação real que será linkada ao link simbólico e, por fim, deve se passar também junto ao argumento **\-\-install**, o nível prioridade que a nova versão terá.

Segue exemplo:

```bash
update-alternatives --install /usr/bin/java java /opt/jdk1.8/bin/java 1
```

Após o comando acima, basta confirmar com **\-\-list**:

```bash
update-alternatives --list java
```

Para escolher a nova opção, passando o número da seleção, execute o comando com argumento **\-\-config**, o qual disponibilizará um menu de escolha de forma intuitiva:

```bash
update-alternatives --config java
```

Há também a forma mais direta de se alternar a versão, passando o argumento **\-\-set** ou **-s**, conforme abaixo:

```bash
sudo update-alternatives --set java /opt/jdk1.8/bin/java
```

Para maiores informaçoes:

```bash
man update-alternatives
```

