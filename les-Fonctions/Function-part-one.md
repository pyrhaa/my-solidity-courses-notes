#### Fontion part.1

Dans cette partie, nous nous concentrerons sur les sujets suivants liés aux fonctions:

- Visibility and Getters
- Function Modifiers
- Constant and Immutable State Variables
- Function Parameters and Return Variables
- Pure, View Functions

---

##### Visibility and getters

Il existe quatre types de visibilité pour les fonctions et les state variables; `external`, `public`, `internal` et `private`.

> `external` visibility n'est pas possible pour les state variables.

> > Notes: Tout ce qui se trouve à l'intérieur d'un contrat est visible par tous les observateurs extérieurs à la blockchain. Rendre quelque chose `private` empêche seulement les autres contrats de lire ou de modifier les informations, mais il sera toujours visible par le monde entier en dehors de la blockchain.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract A {
    // only visible to contract A
    uint private index = 1;
    // visible to contract A, also visible to derived-inherited contracts such as B
    mapping(uint => uint) internal ids;


    /**
    * internal function: can be invoked within contract A
    * or from contract B which inherits this contract
    */
    function idGenerator() internal {
        ids[index++] = _idGenerator();
    }

    /**
    * @dev Warning: it isn’t an easy task to generate true random input.
    * Do not rely on block.timestamp and block.difficulty as a source of randomness.
    * Since these values can be manipulated by miners.
    *
    * private function: only visible inside the contract A
    */
    function _idGenerator() private view returns (uint) {
      return uint(keccak256(abi.encode(block.timestamp, block.difficulty)));
    }
}

// contract B inherits contract A
contract B is A {
    // public state variable, visible to any place
    address public owner;
    /**
    * Getter function generated automatically by the compiler for public state variables
    *
        function owner() public view returns (address) {
            return owner;
        }
    *
    */

    constructor() {
        owner = msg.sender;
    }

    /**
    * public function: can be invoked from any place
    *
    */
    function isOwner() public view returns (bool) {
        return msg.sender == owner;
    }

    /**
    * external function: can be called from other contracts
    * not from contract B
    */
    function generateId() external {
        idGenerator();
    }

    function test() external view {
        index++; // illegal: index is not visible, private variable
        owner = address(0x00); // legal: owner is public
        uint x = ids[1]; // legal: ids mapping is internal, visible to derived contracts
        generateId(); // illegal: generateId is marked as external, can be called from outside
    }
}
```

---

##### Function Modifiers

En utilisant les modifiers, nous pouvons vérifier une condition avant d'exécuter la fonction. Ce sont des propriétés héritables des contrats et peuvent être remplacées par des contrats dérivés, mais seulement s'ils sont marqués `virtual`.

> > Notes: Nous pouvons appliquer plusieurs modifiiers aux fonctions en les plaçant dans une liste séparée par des espaces et ils sont évalués dans l'ordre présenté.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Modifiers {
    address public owner;
    uint private index = 1;
    mapping(uint => uint) internal ids;

    // modifier for checking whether message caller is owner or not
    modifier onlyOwner() {
        require(msg.sender == owner, 'Only owner!');
        _;
    }

    // setting msg.sender as the owner
    constructor() {
        owner = msg.sender;
    }

    /**
    * applying modifier, only owner can execute this function
    *
    * we can use 'onlyOwner' instead of 'onlyOwner()', both are same
    */
    function idGenerator() internal onlyOwner() {
        ids[index++] = _idGenerator();
    }

    /**
    * private function: only visible inside contract Modifiers
    *
    */
    function _idGenerator() private view returns (uint) {
      return uint(keccak256(abi.encode(block.timestamp, block.difficulty)));
    }

    /**
    * applying multiple modifiers
    *
    * modifier a() {...}
    * modifier b() {...}
    *
    * function foo() public a() b() {...}
    */
}
```

---

##### Constant and Immutable State Variables

`immutable` et `constant` sont des keywords qui sont usités sur des state variables pour restreindre les modifications à leur state. La différence est que `constant` variables ne peuvent jamais être modifiées après la compilation, tandis qu'`immutable` variables peut être défini dans le constructor. Il est également possible de déclarer des variables constantes au niveau du fichier.

> > Notes: Le compiler ne réserve pas un **storage slot** pour ces variables, et chaque occurrence est remplacée par la valeur respective.
