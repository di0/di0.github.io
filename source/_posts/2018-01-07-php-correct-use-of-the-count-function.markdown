---
layout: post
title: "PHP - Correct use of the count function"
date: 2011-12-02 23:10:47 -0200
comments: true
categories: [php]
---

Comumente e erroneamente, a função **count** é chamada dentro de um laço de repetição, na
intenção de impor um limite de contagem em uma determinada rotina de repetição, um simples
exemplo desse caso seria:

```php
$alfa = array( 1, 2, 3, 4, 5 );

for ( $i = 0; $i <= count($alfa); $i++ )
       // codigo....
```

Veja que desnecessariamente, chama-se a cada interação da instrução for, a função count().

No contexto de se impor um limite, a forma correta seria fora do laço de repetição, chamar uma única vez a
função, atribuindo seu valor de retorno em uma variável e essa sim, será utilizada dentro da instrução de
repetição, segue:

```php
$alfa = array( 1, 2, 3, 4, 5 );

$count = count( $alfa );

for ( $i = 0; $i <= $count; $i++ )
      // codigo.... 
```
