## Reference types data location

Tout reference type a une annotation supplémentaire (la data location) sur l'endroit où elles sont stockées. Il y a trois options possibles : memory, storage et calldata.

- `storage` : le type d'emplacement où les state variables sont stockées sur la blockchain de manière permanente, ce qui signifie que les types qui ont un `storage` location sont persistants.

- `memory` : les variables qui sont stockées en `memory` existent pendant l'appel de la fonction, ce qui signifie que les valeurs qui ont cet emplacement sont temporaires et qu'une fois l'exécution de la fonction terminée, elles n'existeront plus.

- `calldata`: non-modifiable, non-persistent data location où les function arguments sont stocké, se comporte principalement comme la data location `memory` et n'est disponible que pour les `external` functions.

---

L'emplacement de l'ensemble de données est important non seulement pour la persistance des données, mais aussi pour la sémantique de l'assignement. Regardons chaque comportement ;

- l'assignement entre `storage` et `memory` (ou depuis `calldata`) crée toujours une copie indépendante.

- l'assignement depuis `memory` vers `memory` crée seulement des references.
  Par conséquent, les modifications apportées à une memory variable sont également visibles dans toutes les autres memory variables faisant référence aux mêmes data.

- l'assignement depuis `storage` vers une local `storage` variable aussi assigne seulement une reference.

- tout les autres assignements vers `storage` crée toujours des copies indépendantes.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.9.0;

contract DataLocations {
    // state variables also we can call them storage variables
    // only place that you don't need to define data location
    // below variables are persistent and stored on blockchain
    address[] public addresses;
    mapping(uint => address) owners;

    struct Payment {
        uint amount;
        uint timestamp;
        address payable[] receiver;
    }
    mapping(uint => Payment) payments;

    function test() public {
        // calling _storageOps function with state aka storage variables
        _storageOps(addresses, owners, payments[1]);

        // storage to storage
        Payment storage paymentOne = payments[1];

        // create a struct in memory, non-persistent variable
        // it will be available during 'test' function execution
        Payment memory paymentTwo = payments[2];

    }

    /**
    * @notice we are making operations on storage variables,
    * persistent effect
    *
    */
    function _storageOps(
        address[] storage _addresses,
        mapping(uint => address) storage _payments,
        Payment storage _payment
    ) internal {
        // operation on storage variables
        // ...
    }

    /**
    * @notice remember, all reference types
    * needs data location assigment
    *
    */
    function memoryOps(bytes32[] memory _ids) public returns (uint[] memory) {
        // You can return memory variables
        // operations on bytes32 array
        // ...
    }

    /**
    * @notice since it is external function,
    * it is needed to specify 'calldata'
    *
    * @param _arr, non persistent, non modifiable variable,
    * only exist during 'foo' function lifetime
    */
    function foo(uint[] calldata _arr) external {
        // do something with calldata array
    }
}
```
