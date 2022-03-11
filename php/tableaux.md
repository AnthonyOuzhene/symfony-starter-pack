# Les tableaux

On peut utiliser les tableaux dans deux cas :

- structurer des données liées entre elles

```php
$pizza4Fromage = [
    'pate' => 'fine',
    'topings' =>  ['fromage1', 'fromage2', 'fromage3', 'fromage4', 'tomate'],
];

$pizzaHawaienne = [
    'pate' => 'epaisse',
    'topings' => ['fromage', 'tomate', 'ananas'],
]
```

- ou lister des données similaires


```php
$listePrenoms = ['Pierre', 'Paul', 'Jacques'];

// et on peut lister d'autres tableaux !
$cartePizzeria = [
    $pizza4Fromage, 
    $pizzaHawaienne, 
    [
        'pate' => 'pan', 
        'topings' => ['merguez', 'agneau', 'fromage', 'cheddar'],
    ]
];
```
