# Tests unitaires avec PHPUnit et Symfony

Ces tests garantissent que les unités individuelles du code source (par exemple une seule classe) se comportent comme prévu.

PHPUnit fournit tout une suite de classes et méthodes utiles pour faciliter l'écriture de tests.
[Documentation PHPUnit](https://phpunit.de/)

## Installation dépendance (via composer)

- Lancer la commande suivante pour installer phpunit & phpunit-bride & créer le dossier de test :

```sh
composer require --dev symfony/test-pack
```

- Si installation par package :

```sh
composer require --dev phpunit/phpunit
composer require --dev symfony/phpunit-bridge
```

Attention aux concordances de versions entre PHP et PHPUnit :

- à partir de la version 6.x de PHPUnit, il faut que la version de PHP installée soit au moins à 7.x ;
- la version 5.7 de PHPUnit requiert au minimum la version 5.6 de PHP.

## Installation globale (sous Linux => mettre à jour la version)

```sh
$ wget https://phar.phpunit.de/phpunit-9.6.8.phar
$ chmod +x phpunit-9.6.8.phar
$ sudo mv phpunit-9.6.8.phar /usr/local/bin/phpunit
$ phpunit --version
PHPUnit 9.6.8 by Sebastian Bergmann and contributors.
```

## Vérification installation PHPUnit

1. Dans le terminal, à la racine du projet, lancer la commande suivante :

```sh
vendor/bin/phpunit
# OU
phpunit # si installation en global
```

2. Réponse attendue :

`No tests executed!`

## Lancer des test PHPUnit

### Créer la classe de Test

Tous les tests doivent être écrits dans des classes de Test. dans un projet Symfony :

- cette classe doit être contenu dans le dossier tests du projet
- il faut reproduire l'arborescence de la classe que vous souhaitez tester
- la classe de test doit avoir le même nom que la classe de test, suffixé par Test.
- Nommer bien les méthodes de test. Leur nom doit être facilement compréhensible pour d'autres développeurs, afin qu'en un coup d'œil, on puisse comprendre ce qui est testé.

Exemple :

1. Créer la classe Product dans le dossier src/Entity

```php
<?php

namespace App\Entity;

use Symfony\Component\Config\Definition\Exception\Exception;

class Product
{
    const FOOD_PRODUCT = 'food';
    public function __construct($name, $type, $price)
    {
        $this->name = $name;
        $this->type = $type;
        $this->price = $price;
    }

    public function computeTVA(): float | Exception
    {
        if (self::FOOD_PRODUCT == $this->type) {
            return $this->price * 0.055;
        }
    }
}
```

2. Créer la classe ProductTest dans le dossier tests/Entity

```php
<?php
# Le namespace doit être le même que la classe à tester
# Il faut faire appel à la classe TestCase du code source de PHPUnit avec le use
# Il faut faire appel à la classe à tester dans le dossier Entity avec le use
namespace App\Tests\Entity;
use PHPUnit\Framework\TestCase;
use App\Entity\Product;

class ProductTest extends TestCase
{
        public function testDefault()
    {
        $product = new Product('Pomme', 'food', 1);
        $this->assertSame(0.055, $product->computeTVA());
    }
}
```

3. Lancer les tests avec phpunit

<img src="../images/1st-test.png" alt="Résultat du test" />


## Générer un rapport de couverture de code

Un rapport de couverture de code est un rapport qui permet de savoir quelles parties du code sont couvertes par les tests et quelles parties ne le sont pas.
Il suit un taux de couverture de code, qui est le pourcentage de code couvert par les tests.

1. Lancer la commande suivante :

```sh
# Générer un rapport de couverture de code en texte dans terminal
XDEBUG_MODE=coverage ./vendor/bin/phpunit --coverage-text
# OU
# Générer un rapport de couverture de code en html
vendor/bin/phpunit --coverage-html public/test-coverage
```

2. Si l'erreur `No code coverage driver is available` apparaît, il faut installer xdebug :

```sh
sudo apt-get install php-xdebug
```

3. Paramétrer le fichier php.ini (ou xdebug.ini si un fichier est dédié à xdebug) :
<!-- `php --ini` pour connaître le chemin du fichier php.ini -->

- Dans le terminal faire `sudo nano /etc/php/8.2/mods-available/xdebug.ini`
- ajouter la ligne suivante : xdebug.mode=coverage
- sauvegarder le fichier

4. Relancer la commande `vendor/bin/phpunit --coverage-html public/test-coverage`.

5. lancer la commande `ls -lah public/test-coverage` pour vérifier que le dossier test-coverage a bien été créé.

5. Ouvrir le fichier public/test-coverage/index.html dans le navigateur.

- cliquer sur le dossier pour consulter le code de l'application
- si une ligne est verte, le code est correctement exécuté par les tests
- si une ligne est rouge, le code n'est pas exécuté par les tests
- Si une ligne est orange, le code est incomplet

## Réglage comportement PHPUnit

Le fichier de configuration `phpunit.xml.dist` peut être modifié pour :

- afficher des couleurs lors des tests
- choisir le dossier de tests
- etc.
