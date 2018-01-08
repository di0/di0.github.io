---
layout: post
title: "Creating objects in Perl"
date: 2012-05-11 22:08:36 -0200
comments: true
categories: [perl]
---

É possível trabalhar com objetos em Perl, não muito semelhantes como encontrado linguagens como Java, C++ e outras, o conceito de orientação à objetos em Perl, são resumidamente representados por Hash e Bless.

Uma Classe, é representada como um Pacote, utilizando a palavra chave package para criá-la:

```perl
 package Pessoa;
```
Quando um Pacote está no mesmo arquivo, é necessário um pacote principal:

```perl
package main;
```

Método Construtor é feito através da palavra chave new:

```perl
package Pessoa;
 
sub new
{
	my $this = shift;
	my $class = ref($this) || $this;
    
	my $self = {
		nome => shift,
		ano_nasc => shift
	};
 
	return bless $self, $class;
}
```
