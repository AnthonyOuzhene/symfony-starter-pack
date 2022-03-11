### Forms


[La doc](https://symfony.com/doc/current/forms.html)


```sh
# installation du composant :
composer require symfony/form
```
```sh
# - Créer la classe de formulaire dans le dossier Forms :
bin/console make:form
```

## Dans le Controller

- créer l'objet formulaire et le founir à la vue (avec `$this->renderForm`)


## Dans la vue Twig

- Afficher le formulaire grace aux fonctions form_ [Cf La doc](https://symfony.com/doc/current/form/form_customization.html)

## Traitement du formulaire ( retour dans le meme controller )

A chaque formulaire, pensez à suivre les étapes suivantes :

1. Vérification des données
2. Valider les donées
3. Traiter les données
4. Message Flash
5. Redirection

on ajoute la partie :

```php
$form->handleRequest($request)
if($form->isSubmitted() && $form->isValid)
{
    // traitement du formulaire 
    // redirection
}
```

## validation du formulaire

```sh
# installer le composant :
composer require symfony/validator
```
