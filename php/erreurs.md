# Les erreurs communes dans PHP

et comment s'en dépatouyer

## Anatomie d'une erreur PHP

Une erreur PHP aura ce format
```
Notice: Undefined index: name in /var/www/html/zeus/spe/symfo-e01-rappels-gregoclock/application/templates/contactShow.tpl.php on line 5
``` 

Notice : le type de l'erreur, cela peut etre Warning, Error, Parse Error par exemple
Undefined index: name : l'erreur rencontrée
in /var/.../contactShow.tpl le fichier où PHP a rencontré l'erreur
on line 5: et la ligne !


## Les erreurs d'analyse ( aka PARSE ERROR)

### Syntax error

> unexpected QUELQUE CHOSE

php a rencontré "QUELQUE CHOSE" d'innatendu et il y a un manque dans le fichier.
Pense à vérifier tes `;` et autres fermetures de `)`, `}`, `]`, `'` et `"`

### Fatal error

> Class 'UN NOM DE CLASSE' not found

PHP n'a pas connaissance de cette classe

Pense à vérifier le require ou le use.