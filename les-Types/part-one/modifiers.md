## Modifiers pour fonctions

Une fonction de type `A` est implicitement convertible en une fonction de type `B` si et seulement si
leurs types de paramètres sont identiques, leurs types de return sont identiques,
leur propriété interne/externe est identique et la mutability state de `A`
est plus restrictive que celle de `B`.

- `pure` interdit à notre fonction de lire ou de modifier le state de notre contrat.

- `view` autorise la lecture du state de notre contract mais qui interdit la modification

- `payable` autorise la réception d'Ether avec un appel.

## Modifiers state variables

`immutable` et `constant` sont des mots-clés qui peuvent être utilisés sur les variables d'état pour restreindre les modifications de leur state. La différence est que les variables constantes ne peuvent jamais être modifiées après la compilation, tandis que les variables immuables peuvent être définies dans le constructeur. Il est également possible de déclarer des variables constantes au niveau du fichier.

- `constant` non modifiable après compilation
- `immutable` Permet exactement un assignement dans le constructor et est constant par la suite. Est stocké dans le code.

## Modifiers pour events

- `anonymous` Ne stocke pas la signature d'event en tant que sujet.

- `indexed` pour event parameters: stocke le paramètre en tant que sujet.
