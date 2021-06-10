## References types

Dans cette partie, nous examineronsles references types - `arrays`, `struct` et `mapping` qui stockent l'emplacement des données, une <em>reference</em>, et ne partage pas la data directement.
Il est évident qu'il faut manipuler ce genre de types avec plus de prudence puisqu'il s'agit d'emplacements , sinon on peut perdre facilement en performances.

---

**Arrays** sont un groupe d'éléments du même data types dans lequel chaque élément a un emplacement particulier appelé index. La taille des arrays peut être fixe ou dynamique.

> Dans Solidity, A[5] est toujours un array contenant cinq éléments de type A, même si A est lui-même un array.

Les **arrays** ont les membres suivants — `length`, `push()`, `push(x)`, `pop()`.
Regardons chacun d'eux avec quelques exemples ci-dessous;

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Arrays {
    uint[5] private amounts = [1, 2, 3, 4, 5]; // fixed sized array of unsigned integers
    uint[] private ids; // dynamic sized array of unsigned integers

    /**
    * @dev returns the element with the given index
    */
    function get(uint index) external view returns (uint){
        return ids[index];
        // return amounts[index];
    }

    /**
    * @dev initialy ids array have no element,
    * beside length of amounts array will be fixed and equal to 5.
    */
    function getLength() external view returns (uint, uint) {
        return (ids.length, amounts.length); // returns tuple (0, 5)
    }

    function getAll() external view returns (uint[] memory, uint[5] memory) {
        return (ids, amounts); // returns tuple ([], [1, 2, 3, 4, 5])
    }

    function add(uint id) external {
       // push(id) append a given id to the end of the ids array
       // push(id) function returns nothing
       ids.push(id);

        /**
        * be careful that we can't push and pop from fixed size array
        * amounts.push(6); // does not work
        */

        /**
        * uint x = ids.push();
        * push() appends a zero-initialzed element at the end of the array
        * and returns a reference to the element
        */
    }

    function remove() external returns (uint) {
        // removes an element from the end of array
        ids.pop();

        return ids.length;
    }

    // array of fixed sized bool arrays
    bool[2][] flags; // i.e. [[ false, true ], [ true, true ]]
}
```

---

**Structs** permettent de déclarer de nouveaux types sous forme de structs.
Ce type est un groupe de différent types , qui peuvent contenir à la fois des value types et des reference types.
Avec une restriction nécessaire — il n'est pas possible pour une `struct` de contenir un membre de son propre type car sa taille doit être finie.
Cela ne signifie pas que struct ne peut pas contenir de struct, par exemple `struct A` peut contenir `struct B` mais `struct A` ne peut pas contenir son propre type `struct A`.

**Mapping** stocke les données dans une <em>key-value pair</em> où une key peut être n'importe quel value types. Nous pouvons penser ce type de référence comme un dictionnaire, où les données peuvent être récupérées par clé(key).

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Example1 {
    // general convention: struct names begin with uppercase letter
    struct Payment {
        uint id;
        address payable recipient; // remember: payable keyword means address can receive ether
        uint amount;
        uint time;
    }

    // mapping(key => value) mappingVariableName
    mapping(uint => Payment) public payments;
    uint public paymentId = 1;

    /**
    * @dev Struct types can be used inside mappings and arrays and
    * they can themselves contain mappings and arrays.
    *
    * Below function creates a new instance from Payment Struct
    *
    */
    function createPayment(
        address payable _recipient,
        uint _amount
    ) external {
        payments[paymentId] = Payment(
            paymentId,
            _recipient,
            _amount,
            block.timestamp
        );

        paymentId++;
    }
}
```

Les **structs** fonctionnent très bien avec les **mappings** et les **arrays**.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Example2 {
    // STATE VARIABLES : more on later
    struct Ticket {
        uint id;
        address buyer;
    }

    // array of Ticket struct instances
    Ticket[] public tickets;

    // mapping reference type can contain another mapping
    mapping(address => mapping(uint => bool)) public ticketOwner;

    /**
    * @dev we calculate a ticketId with another function named _randomNumber
    *
    * carefully look at how we are using mappings
    */
    function reserveTicket() public payable returns (uint) {
        uint ticketId = _randomNumber();
        tickets.push(Ticket(ticketId, msg.sender));

        ticketOwner[msg.sender][ticketId] = true;

        return ticketId;
    }

    /**
    * @dev Warning: it isn’t an easy task to generate true random input.
    * Do not rely on block.timestamp and block.difficulty as a source of randomness.
    * Since these values can be manipulated by miners.
    *
    * A good solution includes a combination of several pseudorandom data inputs and
    * the use of oracles or smart contracts to make it more reliable.
    *
    * You need to be 100% sure nobody can tamper with the data
    * that’s being inputted into your smart contract.
    *
    * We will look at oracles later
    */
    function _randomNumber() internal view returns (uint) {
        return uint(keccak256(abi.encode(block.timestamp, block.difficulty)));
    }
}
```

---

Tout reference type a une annotation supplémentaire, <em>data location</em>, sur l'endroit où elles sont stockées. Il y a trois options possibles : `memory`, `storage` et `calldata`.
