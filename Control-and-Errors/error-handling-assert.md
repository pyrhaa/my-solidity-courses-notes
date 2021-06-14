#### Panic via `assert`

`assert` ne doit être utilisé que pour tester les erreurs internes et vérifier les invariants. Il devrait également retourner true car l'échec de l'assertion signifie qu'il y a un bug dans le code. Voici les cas où Solidity crée un **assert type** — `Panic(uint256)` exceptions;

- Appeler `assert` avec un argument indiquant `false`

- À condition que l'opération arithmétique entraîne un dépassement inférieur(underflow) ou supérieur(overflow).

- Diviser ou modulo par zéro

- Convertir une valeur trop grande ou négative à enum

- Call `pop()` sur un array vide

- Access un array element qui est hors limites ou négatif

- Allouer trop de mémoire ou créer un array trop grand

- Invocation d'une variable initialisée à zéro d'une internal function type

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Assert {
    // Adds two numbers
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;

        // assert should be used for internal error checking
        // throws on overflow
        assert(c >= a);

        return c;
    }
}
```
