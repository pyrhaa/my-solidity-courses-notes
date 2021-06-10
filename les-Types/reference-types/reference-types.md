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
