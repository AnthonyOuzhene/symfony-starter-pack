# Vocabulaire

## les variables

Une variable c'est une variable.

Une variable dans la signature d'une fonction => paramètre d'une fonction

```php
function somme ($a, $b);
```

Une variable dans l'appel d'une fonction => argument

```php
$a = 2;
somme($a, $a); // <-- la valeur de $a est passée en argument
```

Une variable dans une classe => propriété

```php
class A {
    private $uneVariable; // <- c'est une propriété 
}
```

une variable dans une classe qui ne peut changer pas de valeur => constante

```php
class B {
    const JE_NE_CHANGE_PAS_DE_VALEUR = 'Na !';
}
```

## les fonctions

Une fonction c'est une fonction

```php
function somme ($a, $b); // <- une fonction
```

Une fonction dans une classe => méthode

```php
class A {
    private faireUnePizza(); // <- c'est une méthode
}
```

Dans le cadre la fonction qui s'exécute lorsque l'on appelle une route => une action
