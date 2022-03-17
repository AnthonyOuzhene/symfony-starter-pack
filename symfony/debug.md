# Debug : générer du code grace à la console Symfony

## Installation du composant

- Commande de débug (dump())
```sh
composer require --dev symfony/debug-bundle
```

## Au niveau des routes

```sh
# Permet de vérifier la liste des routes
bin/console debug:router
```

## Liste de toutes les classes dans le conteneur de service

```sh
# Permet de vérifier la liste des routes
bin/console debug:autowiring
```

## Liste des events disponibles

```sh
app/console debug:event-dispatcher
```