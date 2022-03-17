# Tests

De manière général, des que je fais un nouveau code ou une modification de code => vérification que ça marche bien

Coder des tests Unitaire : Ecrire du code pour dire à la machine de tester tel ou tel code

## TDD : testDriven Developement

Test Unitaire en centre du code

## Tests unitaires

Symfony intègre le framework **PHPUnit** qui permet d'écrire des tests de manière indépendante.

- Objectif : Tester des Services que l'on va écrire. Et non les Controllers ou les Voters

```sh
# installation du composant
composer require --dev symfony/test-pack
```
Après l'installation, utiliser la commande suivant pour essai

```sh
php ./vendor/bin/phpunit
./bin/phpunit
```


### Création d'un test

```sh
bin/console make:test
```






## Tests fonctionnels
