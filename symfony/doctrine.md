# Doctrine

Doctrine est un ORM ( Object Relational Mapper ) qui permet de relier notre BDD à nos objets PHP.

Quelques fonctionnalités d'un ORM :

- effectuer des requêtes dans la BDD
- analyser nos objets pour mapper ( = associer ) les propriétés des objets et les champs de la BDD.

## Installation

- [La documentation de Doctrine dans Symfony](https://symfony.com/doc/current/doctrine.html#installing-doctrine)
- [La documentation du projet Doctrine](https://www.doctrine-project.org/)

```sh
# Permet d'installer le package qui installera tous les composants nécessaires à l'utilisation de Doctrine dans Symfony
composer require symfony/orm-pack
composer require webapp
```

## Configuration

Pour utiliser doctrine il faut spécifier une valeur de `connection string` à utiliser dans les variables d'environnement

```sh
# .env.local
# On spéficie le nom de l'utilisateur, son mot de passe, le nom de la BDD, le nom du serveur de BDD ( et son port ) ainsi que la version de mysql utilisée ( pour mysql voir l'exemple ci dessous )
DATABASE_URL="mysql://demo_orm:demo_orm_password@127.0.0.1:3306/demo_orm?serverVersion=mariadb-10.3.32&charset=utf8mb4"
```


### Les commandes courantes pour la BDD

Bien relier la BDD dans le .env.local

### Créer la BDD en tant que dev avec un user admin existant

```sh
bin/console doctrine:database:create
```

### Supprimer la BDD

```sh
bin/console doctrine:database:drop --force
```

## Demande à Doctrine d'ignorer toutes les migrations existantes

```sh
bin/console doctrine:migrations:version --add --all
```