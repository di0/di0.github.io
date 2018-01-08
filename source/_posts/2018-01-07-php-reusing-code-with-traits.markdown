---
layout: post
title: "PHP - Reusing code with Traits"
date: 2012-12-01 22:21:08 -0200
comments: true
categories: [php]
---

Propósito geral dos Traits

A Orientação a Objetos em PHP, não trabalha com conceitos de herança múltipla. Apesar da herança múltipla não fazer
falta alguma em PHP(ou em qualquer outra linguagem), por trazer consigo mais desvantagens do que vantagens, como
implementações difíceis, manutenção trabalhosa, acoplamento forte, baixa coesão e centenas de outros
problemas, há ainda assim, a necessidade de se reutilizar recursos em diferentes locais, evitando rescrever trechos
repetidos de códigos.

Traits são mecanismos que possibilitam o reuso de código, em um contexto relacional não vertical. Com traits, em uma
única classe é possível utilizar recursos que se tornam modulares e que são carregados conforme necessidade. A vantagem de
utilizar **Traits**, vai além da reutilização e, o grande destaque está em seu relacionamento com outros participantes que, ao
contrário da herança, não tem o relacionamento *"É UM"* e sim um talvez, se *"COMPORTA(-SE) COMO"*. Essa sútil diferença é de suma
importância, evitando os problemas citados anteriormente quando se faz o uso indiscriminado e / ou incorreto de uma ou muitas
extensões(o qual já foi citado não ser suportado no PHP), evitando-se relacionamentos excêntricos, *"É UM"*, onde por exemplo
uma classe Pessoa estende Database, formando o relacionamento Pessoa É UM Database, quando Pessoa, obviamente, não É UM Database.

Um exemplo simples para introduzir a sintaxe em torno das Traits:

```php
trait Foo
{
	function firstName()
	{
		return 'James';
	}
}

trait Bar
{
	function lastName()
	{
		return 'Bond';
	}
}
 
class Baz
{
	use Foo, Bar;
}
 
$objBaz = new Baz();
$strResult  = 'My name is ';
$strResult .= $objBaz->lastName();
$strResult .= ", ";
$strResult .= $objBaz->firstName();
$strResult .= " ";
$strResult .= $objBaz->lastName();
$strResult .= "!\n";
echo $strResult;          // My name is Bond, James Bond!
```

Traits assemelham-se às classes, mais aproximadamente às <!--more--> classes abstratas, que não
podem ser instanciadas diretamente, todavia, seu principal objetivo é de reunir 
grupo(s) de comportamentos(s) que serão reutilizadas em um ou diversos pontos.

Visão mais ampla

Um outro exemplo recorrente, ajuda a ilustrar melhor o propósito de Traits:

```php
trait Singleton
{
	private static $instance;
 
	public static function getInstance()
	{
		if ( !( self::$instance instanceof self ) )
		{
			self::$instance = new self;
		}
 
		return self::$instance;
	}
 
	protected function __construct() { }
 
	private function __clone() { }
 
	private function __wakeup() { }
}
 
class DatabaseLoader
{
	use Singleton;
	{...}
}
 
class  FileLoader
{
	use Singleton;
	{...}
}
```

Nesse exemplo, tanto a classe DatabaseLoader, como FileLoader, se comportam como Singleton, e por ambas herdarem
esse comportamento, não é possível(devido aos construtores protegidos) criar novos objetos dessas:

```php
$database = new DatabaseLoader();
$file = new FileLoader();
```

```bash
PHP Fatal error: Call to protected DatabaseLoader::__construct() from invalid context ...
PHP Fatal error: Call to protected FileLoader::__construct() from invalid context ...
```

As classes DatabaseLoader e FileLoader tem comportamentos idênticos à Trait Singleton, portanto, para o efeito
desejado o correto será:

```php
$databaseLoad_1 = DatabaseLoader::getInstance();
$databaseLoad_2 = DatabaseLoader::getInstance();
 
$fileLoad_1 = FileLoader::getInstance();
$fileLoad_2 = FileLoader::getInstance();
 
if ( $databaseLoad_1 instanceof $databaseLoad_2 )
	echo "Singleton is working";
 
if ( $fileLoad_1 instanceof $fileLoad_2 )
	echo "Singleton is working";
```

Isso reforça a grande cartada de Traits – possibilitar comportamentos aos participantes, ou seja, relacionamentos
do tipo comporta-se como. A leitura dessas classes seria algo adjacente a: DatabaseLoader e FileLoader não são
Singletons, não tem Singletons(usar singleton não é um contrato), mas se comportam como Singletons.

Futuramente, tanto DatabaseLoader como FileLoader, de maneira flexivelmente simples, poderiam deixar de se comportar
como Singletons, apenas deixando de usar(se comportar como) Trait Singleton.

Traits e Interface

Um exemplo usando Traits e Contrato:

```php
class CSV
{
        public function Output() {
                // {...}

                return "CSV file contents has been read";
        }
}

//---------------------------------------------------------

class PDF
{
        public function Output() {
                // {...}

                return "PDF file contents has been read";
        }
}

//---------------------------------------------------------

trait LoaderCSV
{
        private $file;

        function __construct( $file )
	{
                $this->file = $file;
        }

        public function read() {
                $csv = new CSV($this->file);

                // {...}

                return $csv->Output();
        }
}

//---------------------------------------------------------

trait LoaderPDF
{
        private $file;

        function __construct( $file )
	{
                $this->file = $file;
        }

        public function read()
	{
                $pdf = new PDF($this->file);

                // {...}

                return $pdf->Output();
        }
}

//---------------------------------------------------------

interface ReaderFile
{
        public function read();
}

//---------------------------------------------------------

class PDFReaderFile implements ReaderFile
{
        use LoaderPDF;
}

//---------------------------------------------------------

class CSVReaderFile implements ReaderFile
{
        use LoaderCSV;
}

//---------------------------------------------------------

class LoaderContents
{
	private $contents;

	public function __construct( ReaderFile $rf )
	{
		$this->contents = $rf->read();
	}

	// {...}

	public function getContents()
	{
		return $this->contents;
	}
}

//---------------------------------------------------------

$filePDF = new LoaderContents(new PDFReaderFile("file.pdf"));
echo $filePDF->getContents() . "\n";  // PDF file contents has been read

$fileCSV = new LoaderContents(new CSVReaderFile("file.csv"));
echo $fileCSV->getContents() . "\n";  // CSV file contents has been read
```

Há alguns pontos a serem ressaltados, por exemplo, como é notado, as classes PDFReaderFile e CSVReaderFile tem
(uma interface) um ReaderFile, mas cada um se comporta(uma trait) de uma maneira. Nesse caso também, foi
possível realizar uma abstração encapsulada, pois, quem usa a classe LoaderContents, não tem conhecimentos de que
o contrato assinado está sob comportamentos ou uma implementação.

Os construtores de cada Traits são herdados, tendo os construtores dos participantes que usam essas
Traits, os mesmos comportamentos.

Traits e Autload

Também é possível utilizar Traits com autoload/spl_autload_register e a função trait_exists():

```php
class ReaderFile
{
 	use LoaderFile;
}

function __autoload( $name )
{
	$trait = $name.".php";

	if ( file_exists( $trait ) )
	{
		include_once( $trait );

		if ( !trait_exists( $name ) )
			throw new exception( "No trait exists" );
        }

	else
		throw new exception("No file exists");
}
```
Conclusão final

Há uma infinidade de maneiras de se usar Traits, como compor Traits dentro de Traits, criando uma hierarquia
de comportamentos. Sem dúvida, Traits trouxe para o PHP muito mais flexibilidade em todos os sentidos.
