# Le principe du TDD

Le principe d'un test unitaire est très simple. Il s'agit :

- d'exécuter du code provenant de l'application à tester.
- Et de vérifier que tout s'est bien déroulé comme prévu.

## Test Driven Development (TDD)

```php
# src/Entity/Product.php
<?php
namespace App\Entity;
class Product
{
    const FOOD_PRODUCT = 'food';
    private $name;
    private $type;
    private $price;
    public function __construct($name, $type, $price)
    {
        $this->name = $name;
        $this->type = $type;
        $this->price = $price;
    }
    public function computeTVA()
    {
       if (self::FOOD_PRODUCT == $this->type) {
          return $this->price * 0.055;
       }
       return $this->price * 0.196;  
    }
}
```

Une indication pour connaître le nombre de tests qu'il va falloir écrire concerne les sorties possibles de la méthode. Dans le code présenté ci-dessus, vous voyez deux  return  :  il va falloir deux tests pour faire en sorte que tous les cas soient couverts.

## Test 1

1/ Créer la classe de test ProductTest dans le dossier tests/Entity

```php
# tests/Entity/ProductTest.php
<?php
namespace App\Tests\Entity;
use App\Entity\Product;
use PHPUnit\Framework\TestCase;
class ProductTest extends TestCase
{
    public function testcomputeTVAFoodProduct()
    {
                $product = new Product('Un produit', Product::FOOD_PRODUCT, 20);
       $this->assertSame(1.1, $product->computeTVA());
       # la méthode assertSame() étendue de la classe TestCase, permet de vérifier que les deux valeurs sont identiques.
       # Elle prend deux paramètres :
         # - la valeur attendue
         # - la valeur retournée par la méthode à tester
    }
}
```	

Il s'agit de :

- Construire l'objet dont nous allons avoir besoin pour appeler la méthode  computeTVA  .
- Puis de faire appel à PHPUnit pour effectuer une assertion.

Une assertion est une vérification qui va permettre de savoir si le test est passé ou non. Dans le cas présent, nous vérifions que la TVA est bien de 1.1 pour un produit alimentaire à 20 euros.
C'est un booléen qui est retourné par la méthode  assertSame(). Si le test est passé, le booléen vaut  true, sinon il vaut false.

2/ Lancer le test avec la commande `vendor/bin/phpunit`
Si tout s'est bien passé les informations suivantes sont ressorties :

```sh
PHPUnit 9.5.10 by Sebastian Bergmann and contributors.
.                                                                   1 / 1 (100%)

Time: 00:00.002, Memory: 12.00 MB

OK (1 test, 1 assertion)
```

- Le point `(.)` correspond au nombre de tests qui ont été exécutés. (ici 1) ;
- La durée d'exécution du test (ici 00:00.002) et la mémoire utilisée (ici 12.00 MB) ;
- `OK` signifie que le test est passé ;
 
 Si le test n'est pas passé, le résultat est le suivant :
 
 ```sh
PHPUnit 9.6.8 by Sebastian Bergmann and contributors.
F                                                                   1 / 1 (100%)

Time: 00:00.002, Memory: 12.00 MB

There was 1 failure:

1) App\Tests\Entity\ProductTest::testcomputeTVAFoodProduct
Failed asserting that 1.1 matches expected 1.196.

/var/www/html/Tests-unitaires/tests/Entity/ProductTest.php:19

FAILURES!
Tests: 1, Assertions: 1, Failures: 1.
```

- `F` signifie que le test a échoué (Failed) ;

## Test 2

Il reste à tester le cas où le produit n'est pas alimentaire (`!="food"`).

1/ Créer une nouvelle méthode dans la classe de test où le second argument de la méthode constructeur de la classe Product sera différent de `Product::FOOD_PRODUCT` :

```php
# tests/Entity/ProductTest.php
<?php

namespace App\tests\Entity;

use PHPUnit\Framework\TestCase;
use App\Entity\Product;

class ProductTest extends TestCase
{
    public function testcomputeTVAFoodProduct()
    {
        $product = new Product('un produit', Product::FOOD_PRODUCT, 20);
        $this->assertSame(1.1, $product->computeTVA());
    }

    public function testcomputeTVANonFoodProduct()
    {
        $product = new Product('un produit', 'pas un produit alimentaire20', 20);
        $this->assertSame(3.92, $product->computeTVA());
    }
}
```
2/ lancer le test avec la commande `vendor/bin/phpunit`

```sh
PHPUnit 9.6.8 by Sebastian Bergmann and contributors.

Testing
..                                                                  2 / 2 (100%)

Time: 00:00.0028, Memory: 8.00 MB

OK (2 tests, 2 assertions)
```

Tous les tests sont passés. Le code est donc fonctionnel.