## Booleans

`bool`: ses valeurs sont toujours `true` et/ou `false`.

Opérateurs logiques:

- `!` (logical negation)
- `&&` (logical conjunction, "and")
- `||` (logical disjunction, "or")
- `==` (equality)
- `!=` (inequality)

## Integers

`int` / `uint`: Integers signés et non signés. Par exemple, `uint256` entier non signé, positif, de 256 bits.

Operateurs:

- Comparisons: `<=`, `<`, `==`, `!=`, `>=`, `>`
- Arithmetic operators: `+`, `-`, `*`, `/`, `%` (modulo), `**` (exponentiation)

Pour un interger de type `X`, on peut utiliser `type(X).min` et `type(X).max` pour accéder à la valeur minimum et maximum représentable par ce type.
