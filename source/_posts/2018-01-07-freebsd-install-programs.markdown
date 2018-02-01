---
layout: post
title: "FreeBSD - Install programs"
date: 2012-01-02 23:38:14 -0200
comments: true
categories: [freebsd]
---

Existem duas formas comuns de se instalar programas no FreeBSD, via Pacotes e via Ports.

Pacotes já estão disponíveis pré-compilados, são pacotes binários, sendo vantajoso na velocidade de instalação, não sendo necessário a utilização de um compilador no <!--more--> ambiente que está sendo instalado.

Instalações via Ports, são realizadas através código fonte. No processo de instalação, os fontes são baixados
e compilados. Obviamente para isso, há a necessidade de um compilador no ambiente de instalação.

Mais trabalhoso e mais lento que a forma anterior, a vantagem nesse processo de instalação, é a possibilidade de
personalizar opções antes das instalações ou na aplicações de patches(às vezes customizados), etc.

Abaixo a forma de instalação de ambos métodos:

Instalando com Pacote

No FreeBSD, temos o utilitário pkg_add(man pkg_add), para instalações de pacotes:

```bash
pkg_add /foo/pacote-2-2.tar.gz
```
Instalando via Ports

O Ports além de outros, dispõe de arquivos Makefiles, customizados para o processo de instalação. A coleção de
ports ou árvores de ports, estão disponíveis através do diretório /usr/ports/.

Caso o Ports não tem sido instalado, é possível fazê-lo através dos utilitários portsnap ou csup. Dentro
do diretório /usr/ports, há inúmeros aplicativos para instalação. No exemplo, a instalação do servidor Proxy Squid:

```bash
cd /usr/ports/www/squid
make instal clean
```
A instalação é realizada pelo utilitário make, que acima, está em sua forma mais simples, podendo variar
conforme necessidades.
