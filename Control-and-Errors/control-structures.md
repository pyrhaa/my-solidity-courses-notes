## Control structures

La plupart des structures de contrôle que nous connaissons depuis **Javascript**, **C** ou **Python** existent également dans **Solidity**.
Il y a `if`, `else`, `while`, `do`, `for`, `break`, `continue`, `return` avec la sémantique commune des langages mentionnées ci-dessus. Regardons un exemple et étudions chacun d'eux :

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract ControlStructures {
    enum State { PENDING, ACTIVE, INACTIVE }

    State public state = State.PENDING;

    /**
    * @dev changing state according to the conditions
    *
    */
    function changeState() external {
        // generating random number between 0 - 99
        uint _temp = _randomModulo(100);

        // if generated number bigger than 50
        // then change state to ACTIVE
        if (_temp > 50) {
            state = State.ACTIVE;
        }
        // if generated number bigger than 90
        // we send 1000 wei from contract to msg.sender
        else if ( _temp > 90) {
            msg.sender.transfer(1000);
        }
        // if none of the conditions above is true,
        // then the else block is executed.
        else {
            state = State.INACTIVE;
        }
    }

    /**
    * @dev Warning: it isn’t an easy task to generate true random input.
    * Do not rely on block.timestamp and block.difficulty as a source of randomness.
    * Since these values can be manipulated by miners.
    *
    * A good solution includes a combination of several pseudorandom data inputs and
    * the use of oracles or smart contracts to make it more reliable.
    *
    * We will look at oracles later
    */
    function _randomModulo(uint modulo) internal view returns (uint) {
        return uint(keccak256(abi.encode(block.timestamp, block.difficulty))) % modulo;
    }

    /**
    * @dev check whether given number divisible by provided divider
    *
    */
    function isDivisibleBy(uint number, uint divider) external pure returns (bool) {
        if (number % divider == 0) {
            return true;
        }

        return false;

        // same functionality with the above
        // return (number % divider == 0) ? true : false;
    }
}
```

Lors de la rédaction de smart contracts, nous pouvons avoir besoin d'effectuer une action spécifique plusieurs fois. Par conséquent, nous utilisons des <em>loops</em> pour répondre à ce besoin.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Loops {
    /**
    * @dev calculate the sum of given integer array
    * with using for loop
    *
    */
    function forLoop(uint[] calldata _arr) external pure returns (uint sum) {
        for(uint8 i = 0; i < _arr.length; i++) {
            // skip next iteration if array member divisible by 2
            if (_arr[i] % 2 == 0) {
                continue;
            }

            // stops iterating the loop if encounter 10 in array
            if (_arr[i] == 10) {
                break;
            }

            sum += _arr[i];
        }

        return sum;
    }

   /**
    * @dev calculate sum of given integer array
    * with using while loop
    *
    */
    function whileLoop(uint[] calldata _arr) external pure returns (uint sum) {
        uint i;

        while(i < _arr.length) {
            sum += _arr[i];
            i++;
        }

        return sum;
    }

   /**
    * @notice same functionality with the above function
    *
    */
    function doWhileLoop(uint[] calldata _arr) external pure returns (uint sum) {
        uint i = 0;
        do {
            sum += _arr[i];
            i++;
        } while (i < _arr.length);

        return sum;
    }
}
```

> Solidity prend également en charge la gestion des exceptions sous la forme de `try/catch` statements, mais seulement pour les appels d'external function et l'appel de contract creation.
