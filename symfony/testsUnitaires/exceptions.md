# Gérer les exceptions dans les tests unitaires

Le nombre de `return` dans une méthode est une indication pour connaître le nombre de `assert` à faire dans un test. Cependant, ce n'est pas la seule manière de sortir d'une méthode. Il y a aussi les exceptions.
Les exceptions sont des erreurs qui peuvent survenir dans le code. Elles sont gérées par le développeur. Il est possible de les déclencher volontairement pour tester le comportement d'une méthode.

## Déclencher une exception

Pour déclencher une exception, il faut utiliser le mot-clé `throw` suivi d'une instance de la classe `Exception` ou d'une classe qui en hérite.

Exemple : Dans le cas où le prix du produit est inférieur à 0, une exception de type `Exception` est lancée.

1/ Ajouter une condition au début de la méthode computeTVA()

```php
# Attention à bien ajouter le use Exception en haut de la classe
    public function computeTVA(): float | Exception
    {
        if ($this->price < 0) {
            throw new Exception("The TVA cannot be negative.");
        }
        //...
    }
<?php
```

2/ Dans le test, on vérifie que l'exception est bien lancée => Si le prix est positif, on doit avoir une exception donc test échoue

```php
    public function testComputeTVAException()
    {
        // Si le prix est positif, on doit avoir une exception donc test échoue
        $product = new Product("test", 'pas un produit alimentaire', 5);
        $this->expectException(('Exception'););
        $product->computeTVA();
    }
```

4/ On obtient le résultat suivant

```bash
PHPUnit 9.5.4 by Sebastian Bergmann and contributors.

..F                                                                 3 / 3 (100%)

Time: 00:00.006, Memory: 6.00 MB

There was 1 failure:

1) App\Tests\ProductTest::testComputeTVAException
Failed asserting that exception of type "Exception" is thrown.

FAILURES!
Tests: 3, Assertions: 3, Failures: 1.
```

5/ Refaire le test pour que l'exception soit bien lancée en rajoutant un prix négatif

```php
    public function testNegativePriceComputeTVA()
    {
        // Si le prix est négatif, l'exception est levée donc le test est réussi
        $product = new Product('un produit', 'pas un produit alimentaire', -20);
        $this->expectException('Exception');
        $product->computeTVA();
    }
```

6/ On obtient le résultat suivant

```bash
PHPUnit 9.5.4 by Sebastian Bergmann and contributors.

...F                                                                3 / 3 (100%)

Time: 00:00.006, Memory: 6.00 MB

There was 1 failure:

1) App\Tests\ProductTest::testNegativePriceComputeTVA
Failed asserting that exception of type "Exception" is thrown.

FAILURES!
Tests: 3, Assertions: 3, Failures: 1.
```

