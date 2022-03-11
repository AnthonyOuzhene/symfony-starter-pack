# La vérité sur include / require / once ou pas

include et require vont inclure le fichier demandé MAIS require va faire un die() si le fichier n'existe pas alors que include affichera un warning et laissera l'exécution continuer.

la version include_once ou require_once permet de préciser à PHP qu'il ne faut exécuter le fichier demandé qu'une seule fois

```php
$a = 1;
include 'increment.php'; // maintenant $a vaut 2
include 'un_fichier_qui_nexiste_pas'; // ici un warning s'affiche car le fichier n'existe pas ! et que c'est un include

require 'increment.php'; // maintenant $a vaut 3
include_once 'increment.php';// ici $a vaut toujours 3 car le dernier include ne sera pas exécuté !

require 'un_fichier_qui_nexiste_pas'; // ici l'exécution s'arrete car le fichier n'existe pas ! et que c'est un require
```

```php
// increment.php
$a++;
```
