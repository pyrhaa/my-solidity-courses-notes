## Gestion d'erreur: -intro

L'annulation des changements d'état pour éviter d'éventuels problèmes est une nécessité lors de l'écriture de Solidity.
Dans cette partie, nous allons travailler sur la façon de gérer les erreurs en utilisant `revert`, `require`, `assert` et `try/catch` pour annuler toutes les modifications apportées à l'état, dans le <em>call</em> en cours.

Actuellement, Solidity prend en charge deux signatures d'erreur :

- `Error(string)` qui est utilisé pour les <em>regular</em> error conditions.

- `Panic(uint256)` qui est utilisé pour les erreurs qui ne devraient pas être présentes dans le bug-free code.
