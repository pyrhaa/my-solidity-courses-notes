## Dynamically-sized array

Variables de type `bytes` et `string` sont des special arrays dans Solidity.

---

#### Bytes

`bytes` sont utilisés pour les données d'octets brutes de longueur arbitraire.
La longueur de `bytes` est variable de 1 à 32 et est traitée comme un array dans Solidity. Nous pouvons `push`, `pop` et obtenir la longueur de `bytes`.

#### Strings

`string` est utilisée pour stocker la longueur arbitraire du character set(UTF-8) égale ou supérieure à un byte. Il a une longueur dynamique et ne peut donc pas être transmis entre les contrats. De plus, il ne permet pas l'accès à la longueur ou à l'index.

Examinons ci-dessous des exemples pour mieux comprendre. Veuillez lire attentivement les commentaires.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Examples {
   /**
    * @return fixed sized byte value and its length
    *
    */
    function getByteValue() public pure returns (uint8, bytes8) {
        bytes8 language = 'Solidity';

        return (language.length, language); // (8, 0x536f6c6964697479)
    }

   /**
    * @dev string is not equal to bytes32 but it is equal to bytes,
    * because its length is dynamic.
    *
    * casting is allowed from string to bytes
    *
    * @return result bytes representation of string
    */
    function stringToBytes( string memory str) public pure returns (bytes memory result){
        result = bytes(str);

        return result;
    }

    // stringToBytes('This is test conversion from string to bytes')
    // result: 0x54686973206973207465737420636f6e76657273696f6e2066726f6d20737472696e6720746f206279746573
    // no lose


   /**
    * @dev casting is not allowed from string to bytes32
    *
    * WARNING: it will cut strings with length more than 32 bytes
    *
    * The conversion of string to bytes32 is possible only using assembly
    * NOTE: more on the assembly later
    *
    * @return result bytes32 representation of string
    */
    function stringToBytes32(string memory str) public pure returns (bytes32 result) {
        bytes memory tempEmptyStringTest = bytes(str);
        if (tempEmptyStringTest.length == 0) {
            return 0x0;
        }

        assembly {
            result := mload(add(str, 32))
        }
    }

    // stringToBytes32('This is test conversion from string to bytes')
    // result: 0x54686973206973207465737420636f6e76657273696f6e2066726f6d20737472
    // previously result: 0x54686973206973207465737420636f6e76657273696f6e2066726f6d20737472696e6720746f206279746573
    // there is a loss

   /**
    * @return string representation of bytes32
    *
    */
    function bytes32ToString(bytes32 _bytes32) public pure returns (string memory) {
        uint8 i = 0;
        while(i < 32 && _bytes32[i] != 0) {
            i++;
        }

        bytes memory bytesArray = new bytes(i);
        for (i = 0; i < 32 && _bytes32[i] != 0; i++) {
            bytesArray[i] = _bytes32[i];
        }

        return string(bytesArray);
    }

    // let's provide stringToBytes32('This is test conversion from string to bytes') result
    // to this function
    // bytes32ToString(0x54686973206973207465737420636f6e76657273696f6e2066726f6d20737472)
    // result: This is test conversion from str
    // original str is : This is test conversion from string to bytes
    // There is a loss
}
```
