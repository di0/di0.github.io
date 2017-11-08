---
layout: post
title: "MultiThreading - Future in Java"
date: 2017-11-06 02:32:36 -0200
comments: true
categories: 
---

Ao realizar tarefas concorrentes em Java, desde as primeiras versões, tem-se disponível a interface Runnable, responsabilizando a quem vai
utilizá-la, a implementação de um único método chamado run().
```java
public interface Runnable {
        public void run();
}
```

Como observado acima através de sua a assinatura, não há uma maneira de recuperar o valor de uma operação ou ainda, nem mesmo é possível
lançar uma exceção.

Para casos em que há tarefas que <!--more--> devolvem informações, será necessário o auxílio de um método ou uma propriedade compartilhada para recuperar
armazenar o valor desejado após a execução de uma tarefa.

Conforme exemplo abaixo:

``` java
class Foo implements Runnable
{
        private String operation ;
 
        public String getOperation() {
             return operation;
        }
 
        public void run() {
            // Stuff
            operation = operation();
        }
}
```

Não há problema algum com o uso do exemplo acima, contudo, existe uma solução mais adequada e viável para esse caso, através da
utilização da interface nomeada Callable.

Callable é uma interface que foi disponibilizada na versão J2SE 5.0, ela oferece um método
chamado call(). Através desse método, agora é possível recuperar valores retornados do tipo Object, ou mais
especificamente, qualquer tipo devido à sua construção genericamente parametrizada.

``` java
public interface Callable {
     V call() throws Exception;
}
```

Pelo fato de não ser possível trabalhar com Callable com auxílio da classe Thread, a qual implementa apenas
Runnable, é usado ao invés disso, o suporte da interface ExecutorService para executar objetos do tipo Callable.

Através de ExecutorService, é utilizada o método submit, que aceita como parâmetro um Callable e retorna uma
interface Future, responsável por disponibilizar resultados através de operações assíncronas, a assinatura do
método submit é descrito abaixo:
``` java
Future submit( Callable task )
```

Quando uma tarefa é executada através de Callable, sendo ela “submitada”, pelo método submit(), é retornado a 
interface Future e, através dela, é usado o método get, para recuperar os valores processados.

O método get é mutuamente excluído, até que a tarefa termine, semântica idêntica ao Join da classe Thread.

Uma demonstração do uso de Callable é exibido no código seguinte. Esse teste se baseia em um processo se
dividir em duas linhas de execuções, e realizar a incrementação do valor e exibi-lo logo em seguida. Por exemplo:

``` java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

class Foo implements Callable
{
        private static int value = 0;

        public Integer call()
        {
                try
                {
                        value++;
                        Thread.sleep(600);
                }
                catch ( InterruptedException e )
                {
                        // stuff
                }

                return value;
        }
}
```

``` java
public class Test
{
        public static void main( String[] args ) throws Exception
        {
                ExecutorService s = Executors.newFixedThreadPool( 1 );
                Callable c1 = new Foo();
                Callable c2 = c1;

                for ( int count = 1; count <= 5; count++ )
                {
                        Future f1 = s.submit( c1 );
                        Future f2 = s.submit( c2 );

                        Integer i1 = f1.get();
                        System.out.println( i1 + " -> Executed by c1 reference" );

                        Integer i2 = f2.get();
                        System.out.println( i2 + " -> Executed by c2 reference");
                }

                s.shutdown();
        }
}
```

No trecho de código acima, na linhas 25 é utilizado o método estático newFixedThreadPool(), da classe Executors. Esse método
cria uma espécie de mina de Threads, e diz a quantidade de tarefas que serão executadas a um
determinado momento. O argumento 1, diz que apenas uma Thread executará por vez.

Na linha 26, a variável c1 referencia um objeto que tem Callable a serem executados e na linha seguinte, uma outra
variável(c2) referencia a mesma instância, para que seja possível demonstrar duas linhas de execuções, atuando na mesma tarefa.

No loop, as duas threads serão “submitadas” e executadas e seus valores recuperadas pelo método get() da
interface Future e, no fim do loop, o serviço executor é finalizado com método shutdown(), o resultado da operação será:

``` bash
1 -> Executed by c1 reference
2 -> Executed by c2 reference
3 -> Executed by c1 reference
4 -> Executed by c2 reference
5 -> Executed by c1 reference
6 -> Executed by c2 reference
7 -> Executed by c1 reference
8 -> Executed by c2 reference
9 -> Executed by c1 reference
10 -> Executed by c2 reference
```

De acordo o que foi passado para o método Executors.newFixedThreadPool( 1 ), apenas uma linha foi
executada por tarefa. Caso o parâmetro seja alterado para 2, a saída será algo como:

``` bash
2 -> Executed by c1 reference
2 -> Executed by c2 reference
4 -> Executed by c1 reference
4 -> Executed by c2 reference
6 -> Executed by c1 reference
6 -> Executed by c2 reference
8 -> Executed by c1 reference
8 -> Executed by c2 reference
10 -> Executed by c1 reference
10 -> Executed by c2 reference
```

