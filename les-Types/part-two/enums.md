## Enums

C'est utilisé pour créer des **user-defined** data types, assigner un nom à une constante qui rend les contrats plus lisible, maintenable et moins sujet aux erreurs.
Les options de `enums` peuvent être représentées par des unsigned integer values commençant à partir de 0.
Ils sont explicitement convertibles vers et à partir de tous les integer types, mais la conversion implicite n'est pas autorisée.
`enums` requiert au moins un membre, et sa valeur par défaut lorsqu'elle est déclarée est le premier membre.
`enums` ne peuvent pas avoir plus de 256 membres.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Enums {
    enum State { PENDING, ACTIVE, CLOSED }

    // default value will be set to State.PENDING
    // below 2 lines are equal!
    State private state = State.PENDING;
    // State private state;

    /**
    * @dev External functions are set on functions that can only be called
    * by external accounts or external contracts.
    *
    * @return uint value of enum, 0
    */
    function getState() external view returns(State) {
        return state;
    }

    /**
    * @dev changing state to ACTIVE
    *
    * now, getState() function will return 1 instead of 0
    */
    function activate() external {
        require(state == State.PENDING, 'Only PENDING transactions!');
        state = State.ACTIVE;
    }

    /**
    * @dev changing state to CLOSED
    *
    * getState() function will return 2
    */
    function close() external {
        require(state == State.ACTIVE, 'Only ACTIVE transactions!');
        state = State.CLOSED;
    }
}
```
