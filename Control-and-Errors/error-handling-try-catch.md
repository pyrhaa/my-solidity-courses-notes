#### try / catch

Les exceptions lors de la création du contrat et des appels externes peuvent être détectées avec le `try`/`catch`.
`try` keyword doit être suivi par expression à un appel d'une external function ou une création de contrat (`new ContractName()`).

L'erreur qui est produite après par `revert("reasonString")` ou `require(false, "reasonString")`
a entraîné l'invocation de la clause catch du type `catch Error(string memory reason)`.

`catch (bytes memory lowLevelData)` est exécuté si la signature d'erreur ne correspond à aucune autre clause, s'il y a eu une erreur lors du décodage du message d'erreur ou si aucune data d'erreur n'a été fournie avec l'exception.

Nous pouvons utiliser `catch {...}` si nous ne sommes pas intéressés par les error data.

Afin d'attraper tous les cas d'erreur, nous devons avoir au moins la clause `catch { ...}`ou la clause `catch (bytes memory lowLevelData) { ... }`.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract A {
    address public admin;

    constructor (address _admin) {
        require(_admin != address(0x00), "Zero address not allowed!");
        assert(_admin != 0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2);
        admin = _admin;
    }

    function testFunction(uint _number) public pure returns (string memory) {
        require(_number != 0, "_number cannot be 0!");
        return "testFunction() is called!";
    }
}


contract B {
    event LogErrorString(string message);
    event LowLevelError(bytes data);

    A instanceofA;

    constructor() {
        instanceofA = new A(msg.sender);
    }


    /**
    * @dev try catch example with external calls
    *
    * externalCall(0) => LogErrorString("external call failed!")
    * externalCall(1) => LogErrorString("testFunction() is called!")
    */
    function externalCall(uint _number) public {
        try instanceofA.testFunction(_number) returns (string memory result) {
            emit LogErrorString(result);
        } catch {
            emit LogErrorString("external call failed!");
        }
    }

    /**
    * @dev try catch example with contract creation
    *
    * contractCreation(0x0000000000000000000000000000000000000000) => LogErrorString("Zero address not allowed!")
    * contractCreation(0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2) => LowLevelError("")
    * contractCreation(0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db) => LogErrorString("Contract A created!)
    */
    function contractCreation(address _admin) public {
        try new A(_admin) {
            emit LogErrorString("Contract A created!");
        } catch Error(string memory reason) {
            emit LogErrorString(reason);
        } catch (bytes memory reason) {
            emit LowLevelError(reason);
        }
    }
}
```
