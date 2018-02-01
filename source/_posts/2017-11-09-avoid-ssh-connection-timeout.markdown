---
layout: post
title: "Prevent SSH connection time out"
date: 2016-02-24 02:36:18 -0200
comments: true
categories: [linux]
---

Conexões estabelecidas de clientes SSHs, são automaticamente desconectadas do servidor SSH quando um <!--more--> tempo limite de conexão é alcançado, como mostrado pela mensagem abaixo:

** *Read from remote host foo.com: Connection reset by peer* **
** *Connection to foo.com closed.* **

Para evitar que o cliente perca conexão com o servidor SSH, por timeout, uma das soluções abaixo são avaliadas:

~~Configuração no servidor~~

``` bash
TCPKeepAlive no
ClientAliveInterval 30
ClientAliveCountMax 100
```

**TPCKeepAlive ( KeepAlive )**, verifica se deve ou não, realizar a verificação se o Socket via cliente e servidor, ainda encontra-se
aberto. Um pacote cru, é enviado e se for recebido, significa que a conexão ainda ativa. A opção padrão para a maioria dos caso, é “YES”.

**ClientAliveInterval** Numero do intervalo em segundos em que o servidor, irá enviar um pacote nulo, conforme descrito
anteriormente, ao cliente. O padrão para a maioria dos casos, é zero.

**ClientAliveCountMax 100** Número máximo de vezes, que será enviado o pacote antes de desconectar do cliente. O padrão para a maioria dos casos, são três.

Reiniciando o servidor para aplicar as configurações modificadas:

``` bash
$ sudo service sshd restart
```

~~Configuração no Cliente~~

Se acaso, não for <!--more--> possível realizar os procedimentos anteriores no servidor, devido a permissão, no lado do cliente, é possível realizar o seguinte procedimento:

Se para todos os usuários do sistema, adicionar aqui

** /etc/ssh/ssh_config **

Ou para um usuário específico, adicionar aqui

** ~/.ssh/config **

A seguinte configuração:

**ServerAliveInterval 30**

Configuração no cliente – linha de comando
E por fim, a outra opção, seria a manual, via linha de comando, através da opção (-o)

``` bash
$ ssh -o ServerAliveInterval=30 user@foo.com
```
