# Le fonctionnement de PHP

PHP ( le programme ) lit les fichiers `.php` en 2 temps.

1. Dans un premier temps il analyse (parse en anglais d'où les parse error) tout le fichier pour vérifier si le code ne contient pas d'erreur de syntaxe ( est ce que les balises / accolades / parenthèses sont bien fermées )
2. Dans un deuxième temps il exécute les instructions demandées

## Remarques

Lors de l'analyse du fichier, les fichiers inclus ne sont pas analysés ! Ils le seront lors de l'exécution !

```php
// fichier a.php
echo 'toto';

// le fichier b.php ne sera analysé que si l'exécution du fichier a se déroule jusqu'à cette ligne
require_once 'b.php';
```
