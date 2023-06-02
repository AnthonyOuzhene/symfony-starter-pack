# Les data providers

Avec les data providers, on va pouvoir tester son code avec une suite de valeurs définies sans avoir à créer une méthode de test différente.

[Documentation Data Providers](https://phpunit.de/manual/6.5/en/writing-tests-for-phpunit.html#:~:text=tests%2C%203%20assertions-,Data%20Providers,-A%20test%20method)

## Ajouter l'annotation + nom de méthode en argument

```php
   /**
   * @dataProvider pricesForFoodProduct
   */
    public function testcomputeTVAFoodProduct()
    {
        $product = new Product('un produit', Product::FOOD_PRODUCT, 20);
        $this->assertSame(1.1, $product->computeTVA());
    }
    public function pricesForFoodProduct()
   {
      return [
         [0, 0.0],
         [20, 1.1],
         [100, 5.5]
      ];
   }
   // …
```

Grâce à l'annotation @dataProvider, PHPUnit peut récupérer les données via la méthode donnée en argument d'annotation. Cette dernière doit retourner un tableau :

- $price = 0 et $expectedTva = 0.0
- $price = 20 et $expectedTva = 1.1
- $price = 100 et $expectedTva = 5.5

## Lancer le test 

`vendor/bin/phpunit --filter=testcomputeTVAFoodProduct`

L'option  `--filter`  permet de ne lancer qu'une méthode de test.

même si nous lançons les tests uniquement pour une méthode de test, celle-ci est bien exécutée 3 fois.

```sh
PHPUnit 9.6.8 by Sebastian Bergmann and contributors.

Testing 
...                                                                 3 / 3 (100%)

Time: 00:00.027, Memory: 8.00 MB
```
