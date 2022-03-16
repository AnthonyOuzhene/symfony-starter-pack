# Maker : générer du code grace à la console Symfony

## Installation du composant

```sh
composer require --dev symfony/maker-bundle
```

## Créer le Controller avec bin/console

```sh
# Génère un Controller & un template associé
`bin/console make:controller`

# Pour générer un Controller sans template
`bin/console make:controller --no-template` 
```

## Création d'une entité

Les entités sont les Models dans le cadre des ORM

```sh
# Permet de configurer notre entité, ses propriétés et les relations entre entités.U
# Une classe Repository est créée automatiquement.
bin/console make:entity
```

```sh
# Regénère le repository de la classe créer à la mano
bin/console make:entity --regenerate
```
## Création d'une fixture

[Voir le fichier Fixtures](symfony/fixtures.md)

```sh
# creer un fichier fixture dans le dossier fixtures
bin/console make:fixture
```

## Création d'un user

```sh
bin/console make:user
```

## Création d'une migration

```sh
# On crée les requetes dans des fichiers `migrations` grâce à la commande :
bin/console make:migration
```

Pour appliquer les requêtes dans notre BDD on joue la commande suivante :
```sh
# vérifie les migrations qui sont déjà jouées en BDD et jouer les migrations manquantes.
bin/console doctrine:migrations:migrate
```
```sh
# retirer les migrations inutiles avant de les supprimer à la main.
# '2' est le nombre de migration que l'on veut remonter pour maj de database`.
bin/console make:migration current-2
```

```sh
# Cette commande nous permet de vérifier les fichiers de mapping et lien avec database :
bin/console doctrine:schema:validate
```

```sh
# Aller vers une migration particulière
bin/console doctrine:migrations:migrate [FQCN de la migration à atteindre]
```
```sh
# avoir l'aide d'une commande particulière ( ajouter --help au nom de la commande )
bin/console doctrine:migrations:migrate --help
```

## Création du formulaire de Login 

```sh
bin/console make:auth
```

## Création d'une class Voter 

```sh
bin/console make:voter
```

## Création d'une commande

```sh
bin/console make:command
```
## Création d'un event

```sh
bin/console make:subscriber
```
## Sinon RTFM

```sh
# lister toutes les commandes
bin/console
```
