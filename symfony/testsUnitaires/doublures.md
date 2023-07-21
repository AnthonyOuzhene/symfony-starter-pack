# Les doublures

## Définition

Une doublure est un élément que l’on crée de toutes pièces pour maîtriser une dépendance externe. Cela permet de ne pas dépendre du bon fonctionnement dee cet élément externe.

## Les différents types de doublures

### Mock

Un mock est une doublure qui permet de simuler le comportement d’un objet. Cet objet est créé à partir d’une classe existante dotn on maîtrise le comportement.
Il permet de vérifier que les méthodes d’un objet sont bien appelées.

- [Documentation PHPUnit Mock](https://phpunit.de/manual/6.5/en/test-doubles.html)

### Dummy

Un dummy est un objet qui remplit un contrat mais qui n’a pas de comportement.
GuzzleHttp est un exemple de dummy.

```php
<?php
namespace App\Tests;
use PHPUnit\Framework\TestCase;
class ClassTest extends TestCase
{
   public function testExemple()
   {
      $client = $this->getMock('GuzzleHttp\Client');
      // …
   }
}
```

### Stub

Un stub est un "dummy" auquel on ajoute un comportement. C'est-à-dire qu'on va lui donner les valeurs qu'il doit retourner lorsqu'elle est appelée.

- Transformation de dummy en stub

```php
<?php
namespace App\Tests;
use PHPUnit\Framework\TestCase;
use Symfony\Component\HttpFoundation\Request;
class ClassTest extends TestCase
{
   public function testExemple()
   {
      $request = new Request();
      $client = $this->getMock('GuzzleHttp\Client');
      // Lorsque la méthode get de la classe est appelée, un objet de type  Symfony\Component\HttpFoundation\Request  devra toujours être retourné.
      $client->method('get')->willReturn($request);
      // …
   }
}
```

- Il est tout à fait possible de faire en sorte que la méthode get retourne à son tour un dummy. Pour ce faire, il suffit de remplacer la ligne :

```php
$request = new Request();
```

par :

```php
      $request = $this->getMock('Symfony\Component\HttpFoundation\Request');
```

## Insérer une assertion dans un mock

Un "mock" est un "stub" qui a des attentes (ou Expectations). C'est-à-dire qu'on va lui donner les valeurs qu'il doit retourner lorsqu'elle est appelée, mais aussi les valeurs qu'il doit recevoir en paramètre.

```php
<?php
namespace App\Tests;
use PHPUnit\Framework\TestCase;
use Symfony\Component\HttpFoundation\Request;
class ClassTest extends TestCase
{
   public function testExemple()
   {
      $request = new Request();
      $client = $this->getMock('GuzzleHttp\Client');
      // Ajout d'une assertion ci-dessous à notre stub pour le transformer en mock
      $client
         // S'exécute une fois
         ->expects($this->once())
         // La méthode get doit être appelée
         ->method('get')
         // Retourne un objet de type Symfony\Component\HttpFoundation\Request
         ->willReturn($request);
      // …
   }
}
```

## Doubler un objet

### Raisons de doubler un objet

1. Lorsque celui-ci est difficile à instancier.

Si vous avez besoin de tester les méthodes d'une classe qui requiert des dépendances difficiles à construire, il est pratique de créer une doublure au lieu d'instancier la dépendance en question.

Prenons l'exemple d'une classe difficile à instancier : `JMS\Serializer\Serializer`.

```php
<?php
namespace JMS\Serialiser;
// …
class Serializer
{
   //…
   public function __construct(
      MetadataFactoryInterface $factory,
      HandlerRegistryInterface $handlerRegistry,
      ObjectConstructorInterface $objectConstructor,
      MapInterface $serializationVisitors,
      MapInterface $deserializationVisitors,
      EventDispatcherInterface $dispatcher = null,
      TypeParser $typeParser = null,
        ExpressionEvaluatorInterface $expressionEvaluator = null
   )
   {
      //…
}
}
```

Le constructeur de cette classe requiert 8 paramètres. Il est donc difficile de l'instancier dans un test unitaire. Il est donc préférable de créer une doublure.

Il est plus simple de créer un "dummy" en demandan à PHPUnit de créer un objet de type `JMS\Serializer\Serializer` sans utilise le constructeur original :

```php
<?php
namespace App\Tests\Folder;
use PHPUnit\Framework\TestCase;
class ExempleClassTest extends TestCase
{
   public function testExemple()
   {
      $serializer = $this
         ->getMockBuilder('JMS\Serializer\Serializer')
         ->disableOriginalConstructor()
         ->getMock();
      $classToTest = new ExempleClass($serializer);
      // …
   }
}
```

2. Pour maitriser le comportement d'une méthode par rapport au code original.

Lors de l'exécution d'un test unitaire, il est possible que l'on ait besoin de tester une méthode qui appelle une autre méthode. Il est alors possible de doubler l'objet et de lui donner un comportement spécifique.

```php
<?php
namespace AppBundle\Security;
class GithubUserProvider extends UserProviderInterface
{
   private $client;
   public function __construct(Client $client)
   {
      $this->client = $client;
   }
   public function loadUserByUsername($username)
   {
    // Requête HTTP GET auprès de GitHub (le service externe dans cette exemple). Or, dans un test unitaire, vous devez faire attention à ne pas être tributaire d'une communication auprès d'un service externe, qui pourrait être défaillante.
    $response = $this->client->get('https://api.github.com/user?access_token='.$username);
      // …
   }
}
```

L'objectif est de doubler l'objet `Client` pour lui donner un comportement spécifique maitriser la variable `$response` dans le test unitaire.

Faisons donc un "stub" de l'objet `Client` :

```php
<?php
namespace App\Tests\Folder;
use App\Security\GithubUserProvider;
use PHPUnit\Framework\TestCase
class GithubUserProviderTest extends TestCase
{
   public function testLoadUserByUsername()
   {
      $response = …; // Ce que l'on souhaite recevoir.
      $client = $this->getMockBuilder('GuzzleHttp\Client')
      // on demande à PHPUnit de ne pas faire appel au constructeur original de la classe
         ->disableOriginalConstructor()
         ->getMock();
      $client
      // Avec la méthode setMethods, on indique quelle méthode existe dans cette classe
         ->method('get')
         // précise bien ce qui doit être retourné, en l'occurrence ce que contient la variable  $response => "Ce que l'on souhaite recevoir".
         ->willReturn($response);
      // …
      $githubUserProvider = new GithubUserProvider($client);
      $githubUserProvider->loadUserByUsername('xxxxx');
      // Assertions du test
      // …
   }
}
```

## Résumé

1. Plus le code à tester contient de dépendances, plus il est difficile de le tester. Il est donc important de réduire le nombre de dépendances.
2. Il est important de ne pas tester les dépendances. Il faut donc doubler les dépendances.
    - Mock est une doublure qui a des attentes.
    - Dummy est un mock qui remplit un contrat.
    - Stub est un dummy auquel on ajoute un comportement.
3. Pour que les tests unitaires soient efficaces, il ne faut pas être tributaire d'une communication auprès d'un service externe, qui pourrait être défaillante. Nous restons maître de nos tests en toutes circonstances.
4. Il faut penser à désactiver le constructeur de l’objet avec la fonction disableOriginalConstructor.

## Prophecy : une autre librairie de doublures

Prophecy est une bibliothèque de double de test PHP. Son objectif est de fournir un moyen simple et intuitif de créer des doubles pour les tests unitaires. Pour ce faire, il utilise la technique de la double d'objet par anticipation.

Autre librarie déjà intégré à PHPunit et simple d'utilisation. Elle offre une manière différente d'appréhender les mocks dans ses tests.

- [Documentation Prophecy](https://github.com/phpspec/prophecy)
