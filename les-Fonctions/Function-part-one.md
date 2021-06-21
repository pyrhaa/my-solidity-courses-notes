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

En utilisant les modifiers, nous pouvons vérifier une condition avant d'exécuter la fonction. Ce sont des propriétés héritables des contrats, et peuvent être remplacées par des contrats dérivés, mais seulement s'ils sont marqués `virtual`.

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

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

// file level constant variable
uint constant X = 32**22 + 8;

contract ConstantAndImmutables {
    string constant TEXT = "abc";
    bytes32 constant MY_HASH = keccak256("abc");
    uint immutable decimals;
    uint immutable maxBalance;
    address immutable owner = msg.sender;

    constructor(uint _decimals, address _reference) {
        decimals = _decimals;
        // Assignments to immutables can even access state variables
        maxBalance = _reference.balance;
    }

    function isBalanceTooHigh(address _other) public view returns (bool) {
        return _other.balance > maxBalance;
    }
}
```

---

##### Function Parameters and Return Variables

Les fonctions peuvent prendre des paramètres typés et se comparer à d'autres langages, les fonctions dans Solidity renvoient également un nombre arbitraire de valeurs en output.

###### function parameter

Les paramètres sont déclarés de la même manière que les variables.

###### return variables

<em>Retrun variables</em> sont déclarés avec la même syntaxe que la déclaration des paramètres après le `returns` keyword et ils peuvent être utilisés comme n'importe quelle autre variable locale. Les noms des variables de retour peuvent être omis.

###### Pure, View Functions

`view`: Les fonctions peuvent être déclarées view auquel cas elles ne modifient pas l'état(state).

Les déclarations suivantes sont considérées comme modifiant l'état :

- Writing to state variables.
- Emitting events.
- Creating other contracts.
- Using `selfdestruct`.
- Sending Ether via calls.
- Calling any function not marked `view` or `pure`.
- Using low-level calls.
- Using inline assembly that contains certain opcodes.

> > Notes: Getter methods sont automatiquement marquées `view`.

`pure`: Les fonctions peuvent être déclarées `pure` lorsqu'elles ne lisent pas ou ne modifient pas l'état.

Les éléments suivants sont considérés comme une lecture de l'état :

- Reading from state variables.
- Accessing `address(this).balance` or `<address>.balance`.
- Accessing any of the members of `block`, `tx`, `msg` (with the exception of `msg.sig` and `msg.data`).
- Calling any function not marked `pure`.
- Using inline assembly that contains certain opcodes.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

// function parameters, return variables and pure-view functions
contract Test {
    // state variables -- storage
    uint private index = 1;
    mapping(uint => uint) internal ids;

    /**
    * one parameter and one return type provided
    *
    * name of return variable is omitted
    *
    * function neither modify the state nor read from state,
    * so marked as pure
    */
    function toAddress(bytes32 _key) external pure returns (address) {
        return address(uint160(uint256(_key)));
    }

    /**
    * multiple parameters and multiple return types provided
    *
    * names of return variables are omitted
    *
    * function neither modify the state nor read from state,
    * so marked as pure
    */
    function get(uint _num1, uint _num2) external pure returns (uint, bytes32) {
        return (_num1++, bytes32(_num2));
    }

    /**
    * no function parameters
    *
    * return variables _id used as a local variable
    *
    * function is reading timestamp and difficulty
    * from blockchain-state, so it is marked as view
    */
    function idGenerator() public view returns (uint _id) {
     _id = uint(keccak256(abi.encode(block.timestamp, block.difficulty)));

     return _id % 100;
    }

    /**
    * function changing state
    *
    * cannot be marked as pure or view
    * since it is modifying 'ids' mapping
    */
    function _idGenerator() internal {
        ids[index++] = idGenerator();
    }
}
```
