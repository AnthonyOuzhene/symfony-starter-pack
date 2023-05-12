# Tests unitaires avec PHPUnit

Les tests garantissent la qualité du code et de l'application.

PHPUnit fournit tout une suite de classes et méthodes utiles pour faciliter l'écriture de tests.
[Documentation PHPUnit](https://phpunit.de/)


## Installation dépendance (via composer)

Si installation du webapp => tous les modules de tests s'installent

Si installation par package :

```sh
composer require --dev phpunit/phpunit
```

Besoin d'une dépendance supplémentaire pour lancer phpunit : phpunit-bridge :

```sh
composer require --dev symfony/phpunit-bridge
```

Attention aux concordances de versions entre PHP et PHPUnit :

- à partir de la version 6.x de PHPUnit, il faut que la version de PHP installée soit au moins à 7.x ;
- la version 5.7 de PHPUnit requiert au minimum la version 5.6 de PHP.

## Vérification installation PHPUnit

1. Dans le terminal, à al racine du projet, lancer la commande suivante :

```sh
vendor/bin/phpunit
```

2. Réponse attendue :

`No tests executed!`

