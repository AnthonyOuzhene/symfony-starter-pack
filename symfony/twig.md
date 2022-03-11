## Twig

C'est le composant de templating par défaut de Symfony
On utilise un template depuis un controleur grâce à

```php
$this->render('le nom du tempplate', $lesDatasPréparéesPourLeTemplate);
```

Les templates sont dans le dossier template

#### Héritage


`{{ }}` : permettent d'afficher du contenu. C'est l'équivalent du echo

`{% %}` : permet d'écrire du code dans twig (condition, boucles, etc.). On y prépare les données qu'on transmet à la vue.

`{# #}` : Commentaires