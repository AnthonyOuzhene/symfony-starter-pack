# La philosophie de Symfony

On fait du PHP avec classe (avec plein de classes)
Un concept = création un composant
Le MVC est déjà en place dans Symfony

## Installation de Symfony

```sh
composer create-project symfony/skeleton
```

## Déplace les fichiers à la racine

```sh
#Attention le .env et le .gitignore ne sont pas déplacés par la commande suivante
mv skeleton/* ./
```

- Pour déplacer .env & .gitignore à la racine :
```sh
mv skeleton/.* ./
```

# Suppression du dossier

```sh
rmdir skeleton/
```

## Liste des composants necessaires (si pas installation de webapp)


`composer require form annotations orm twig asset validator`

`composer require --dev debug-bundle maker profiler faker fixtures`


## Si installation de webpack pour composant

```sh
composer remove encore
composer remove messenger
```

## Lister toutes les commandes

```sh
bin/console
```

### Si un projet est déja installé 

1. Lancer le serveur webphp
2. composer install
3. faire le fichier .env.local et remplir mysql : database
4. faire un create database
5. lancer et appliquer les migrations
6. fixtures load