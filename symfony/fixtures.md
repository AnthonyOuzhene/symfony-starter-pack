### Loading Fixtures (données de test)

[Documentation générale](https://symfony.com/doc/current/DoctrineFixturesBundle/index.html#writing-fixtures)

```sh
# installation du composant :
composer require --dev orm-fixtures
```

```sh
# installation du faker pour générer des fausses données
composer require --dev fakerphp/faker
```

```sh
# update les fixtures dans la BDD
bin/console doctrine:fixtures:load
```

```sh
# aide pour découvrir les commandes d'options
bin/console doctrine:fixtures:load --help
```

```sh
# Ne delete pas mais rajoute les nouvelles entrées au début
bin/console doctrine:fixtures:load --append
```

```sh
# remet les id au début. Seulement si une entité n'a pas de clé étrangère
bin/console doctrine:fixtures:load --purge-with-truncate
```