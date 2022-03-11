# DocBlock

Les docblock nous permettent de documenter notre code.
Les docblocks peuvent être lu par des applications ( comme VSCode par exemple )
Pour PHP les docblocks sont des commentaires et sont donc ignorés

Un DocBlock nous permet de donner des informations sur ce que fait telle ou telle partie de notre code. Ceci est particulièrement utile pour une personne qui arriverait sur le projet, afin de comprendre plus rapidement et facilement ce qui a été fait – mais aussi pour nous-même lorsque nous reprenons notre code 6 mois plus tard 😅
Cela permet également de renseigner l’auto-complétion lorsque vous faites appel à ce code ailleurs dans le programme👌

Que peut-on documenter avec un DocBlock ?
Un DocBlock est toujours associé avec un et un seul élément, cela peut être une :

une variable
une constante
une fonction
une classe
une propriété
une méthode
une interface
un trait

Les DocBlocks se divisent en trois parties :

Le résumé (summary ou short description) apporte une brève description de l’élément, généralement en une seule ligne. C’est comme un slogan. Il se termine soit par un . , soit il est suivi d’une ligne blanche, soit les deux.
La description (description ou long description) apporte davantage d’information sur l’élément. Elle est optionnelle mais s’avère particulièrement utile lorsque l’élément est complexe. Elle se termine lorsque le premier tag est rencontré ou, s’il n’y a pas de tag, à la fermeture du DocBlock.
Les tags et annotations donnent des informations structurelles – méta informations – sur l’élément. Les tags peuvent, par exemple, préciser le type des arguments attendus avec @param ou le type de retour d’une méthode avec @return. Chaque tag est précédé d’un @ et démarre sur une nouvelle ligne.