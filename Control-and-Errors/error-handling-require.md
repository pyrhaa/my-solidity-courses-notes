#### Error via `require`

La fonction `require` est utilisé pour garantir des conditions valides qui ne peuvent pas être détectées avant l'exécution. Cela vérifie les conditions on input, contract state variables ou return values depuis des appels d'external contracts. Cela crée soit un error type de `Error(string)` ou une erreur sans aucune error data. Voici les cas où Solidity crée le **require type** - `Error(string)` exceptions;

- Appeler `require` avec un argument montrant `false`

- Exécution d'un appel d' external function à un contrat qui ne contient aucun code

- Quand le contrat reçoit de l'Ether via une public function sans le modifier `payable` - incluant constructor et fallback function

- Lorsque votre contrat obtient de l'Ether à travers une **public getter** function

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Require {
    // state variables also we can call them storage variables
    address public lawyer;

    // after deployment of contract, msg.sender will be lawyer
    constructor() {
        lawyer = msg.sender;
    }

    /**
    * @dev generates random uint256 number
    *
    * Warning: it isn’t an easy task to generate true random input.
    * Do not rely on block.timestamp and block.difficulty as a source of randomness.
    * Since these values can be manipulated by miners.
    *
    * A good solution includes a combination of several pseudorandom data inputs and
    * the use of oracles or smart contracts to make it more reliable.
    */
    function idGenerator() external view returns (uint) {
        // the account that invokes this function must be a lawyer
        // otherwise, create an error 'Only lawyer!'
        require(msg.sender == lawyer, 'Only lawyer!');

        return uint(keccak256(abi.encode(block.timestamp, block.difficulty)));
    }

   function transfer(address payable _to, uint _value) external returns (bool) {
        // _to can't be zero address
        // no error message provided
        // If you do not provide a string argument to require,
        // it will revert with empty error data
        require(_to != address(0x00));

        _to.transfer(_value);
        return true;
    }
}
```

> **remember:** Vous pouvez éventuellement fournir un message string pour `require`, mais pas pour `assert`.

---

Pour les cas suivants, l'error data venant d'un external call(le cas échéant) sont transmises. Cela signifie qu'il peut soit causer un Error ou un Panic (ou quoi que ce soit d'autre qui a été donné):

- Quand `transfer()` échoue

- Lors de l'appel d'une fonction via un appel de message mais cela ne se termine pas correctement <em>(i.e., it runs out of gas, has no matching function, or throws an exception itself)</em>, sauf lors d'une low level operation `call`, `send`, `delegatecall`, `callcode` ou `staticcall` est utilisé. Les low level operations ne lèvent jamais d'exceptions mais indiquent les échecs en retournant `false`

- Lorsque vous créez un contrat en utilisant le keyword `new` mais que la création du contrat ne se termine pas correctement.
