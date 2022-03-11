# Creer ses propres Services

## Creer un fichier Service en respectant logique PSR-4

- Penser au namespace
- Nommer la class comme le nom de fichier

## Pour créer un Slugger :

- Utiliser la dépendance : `use ymfony\Component\String\Slugger\SluggerInterface`.

- Créer la méthode `__Construct` pour injecter les dépendances necessaire et déclarer les propriétés :

exemple : 
```sh
class MySlugger {

    private $symfonySlugger;
    private $toLower;

    public function __construct(SluggerInterface $slugger, bool $slugToLower)
    {
        $this->symfonySlugger = $slugger;
        $this->toLower = $slugToLower;
    }

    public function slugify(string $strToSlug): string
    {

        $slugifiedString = $this->symfonySlugger->slug($strToSlug);

        if ($this->toLower)
        {
            $slugifiedString = $slugifiedString->lower();
        }

        return $slugifiedString;
    }
}
```

## Utiliser ce Slugger

Il en reste plus qu'a utiliser ce nouveau Service en tant que dépendance dans une autre méthode d'une autre Class (ATTENTION : il faudra faire un use  de cette nouvelle dépendance)