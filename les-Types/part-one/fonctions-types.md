## Function Types

1. Les variables d'un type de fonction peuvent être assignées depuis des fonctions.
2. Les paramètres d'un type de fonction peuvent être usités pour passer des fonctions et retourner les fonctions depuis un appel de fonction.

#### Visibility specifier

Il y en a 4:

- `public`: fonction visible/accessible depuis l'extérieur de notre smart contract et aussi depuis du code interne à notre smart contract. C'est la visibilité la plus permissive.
- `private`: fonction ou variable seulement visible dans le smart où elles sont définies.
- `external`: fonctions externes consistent en une adresse et une signature de fonction et elles peuvent
  être transmis via et renvoyé à partir d'appels de fonction externes.
- `internal`: fonction ou variable seulement visible dans le smart contract courant et dans les smart contracts qui en hérite.

Les types de fonction sont notés ainsi:

    function (<parameter types>) {internal|external} [pure|view|payable] [returns (<return types>)]

Contrairement aux types de paramètres, les types de retour ne peuvent pas être vides - si le type de fonction ne doit rien retourner, l'ensemble de la partie `returns (<return types>)` doit être omise.

Par défaut, les types de fonction sont internes, donc le mot-clé `internal` peut être omis. Notez que cela ne s'applique qu'aux types de fonction.
La visibilité doit être préciser explicitement pour les fonctions définies dans les contrats, elles
n'ont pas de valeur par défaut.
