#### revert

Pour revenir sur un <em>current call</em>, on peut utiliser la fonction `revert` pour générer des exceptions pour le displaying error. Cette fonction créera une `Error(String)` exception qui prend éventuellement un string message contenant des détails sur l'erreur.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Purchase {
    uint price = 2 ether;

    function purchaseItem(uint _amount) public payable {
        if (msg.value != _amount * price) {
            revert("Only exact payments!");
        }

        // Alternative way to do it:
        require(msg.value == _amount * price, "Only exact payments!");
    }
}
```

Si nous fournissons l'erreur string directement, alors ci-dessus deux options de syntaxe sont équivalentes, on peut choisir l'une ou l'autre.

L'exemple au-dessus, `revert(“Only exact payments!”)` return l'hexadécimal suivant comme data de retour d'erreur :

```
0x08c379a0 // Function selector for Error(string)
0x0000000000000000000000000000000000000000000000000000000000000020 // Data offset
0x0000000000000000000000000000000000000000000000000000000000000014 // String length
0x4f6e6c79206578616374207061796d656e747321000000000000000000000000 // String data
```
