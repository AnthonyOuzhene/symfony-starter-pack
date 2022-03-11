# Le typage des données

Depuis une version récente de PHP, il est possible de spécifier le type de données attendu par une fonction ainsi que le type de donnée renvoyé par une fonction

PHP (himself) va vérifier que les valeurs fournies lors de l'exécution sont du bon type ! 

Cela nous permet d'être sur que le futur stagiaire n'essaiera pas d'exécuter.

```php
// le code du stagiaire 
$cooker = new PizzaCooker()
$cooker->cook(3);
```

Alors que la classe définit la méthode suivante :

```php
// ma superbe classe 
class PizzaCooker
{
    public function cook(string $pizzaName, int $count)
    {
        // cooking $count $pizzaName
    }
}
```
