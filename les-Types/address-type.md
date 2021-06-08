## Address types

#### Address

**Address type** se décline en deux choix, qui sont en grande partie identiques :

- `address`: est un type qui correspond à une adresse sur Ethereum. Cette adresse peut être un account, ou celle d'un smart contract.

- `address payable`: Pareil qu' `address`, mais avec les possibilités `transfer` et `send`.

L'idée derrière cette distinction est que `address payable` est une addresse vers laquelle on peut envoyer de l'Ether,
tandis que `address` ne peut pas en recevoir.

#### Globales variables

Il existe des variables et des fonctions spéciales qui existent toujours dans [l'espace de noms global](<https://fr.wikipedia.org/wiki/Espace_de_noms_(programmation)>) et sont principalement utilisées pour fournir des informations sur la blockchain ou sont des fonctions utilitaires à usage général.

- `msg.sender`: est une fonction intégrée([global variables](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#address-related)) à Solidity qui indique l'adresse Ethereum en cours de connexion avec le contrat. Son type est `address payable`.

- `balance`: retourne le solde de l'adresse en wei.

- `transfer`: transfère le montant d'Ether en wei à une `payable address`. La fonction échoue si le solde du contrat en cours n'est pas assez important ou si le transfert Ether est rejeté par le compte destinataire. Elle `revert` en cas d'échec et stoppe le contrat en cours avec propagation de l'erreur.

- `send`: équivalent de bas niveau de transfert. Si l'exécution échoue, le contrat en cours ne s'arrêtera pas avec une exception, mais renverra `false`.

Les conversions implicites de `address payable` en `address` sont autorisées, tandis que les conversions d'`address` à `payable address` doivent être explicites via `payable(<adresse>)`.

#### Address literals

**Address literals** ont la représentation hexadécimale d'une adresse Ethereum contenant 40 caractères (20 bytes) et préfixée par 0x.

Les littéraux hexadécimaux doivent réussir le [**address checksum test,**](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-55.md) sinon ce test produira une erreur.

---

Pour finir, un code d'exemple en guise de résumé.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract Address {
    address public admin;
    // if you need arrays of address payable, the notation is below
    address payable[] recipients;

    constructor() {
        // type of msg.sender is address payable
        admin = msg.sender;
    }

   /**
    * @dev External functions are set on functions that can only be called
    * by external accounts or external contracts.
    *
    * "payable" keyword means function can send ether when it executes,
    * without this, function will reject incoming ether transfer
    */
    function fundContract() external payable {
      require(msg.sender == admin, 'only admin');
    }

   /**
    * @dev A function with a view modifier specifies that the function
    * will never “modify state”.
    */
    function balanceOf() external view returns(uint) {
        return address(this).balance;
    }

   /**
    * @dev sending 1000 wei via transfer function to the recipient from the contract.
    *
    * If transfer is failed, propagation of error is occurs and whole transfer will be cancelled
    */
    function sendEtherViaTransfer(address payable recipient) external {
        recipient.transfer(1000);
    }

   /**
    * @dev sending 1000 wei via send function to the recipient from the contract.
    *
    * In case of failure it returns false, no propagation of error occurs.
    */
    function sendEtherViaSend(address payable recipient) external {
        recipient.send(1000);
    }

   /**
    * @dev conversion examples.
    * @param wallet1 plain address
    * @param wallet2 address payable, can receive ether.
    *
    */
    function typeConversion(address wallet1, address payable wallet2) external {
        wallet1 = wallet2;
        wallet1.transfer(1000); // error: transfer is only available for type address payable

        // how can we achieve the conversion from address to address payable?
        address payable newWallet = payable(wallet1);
        newWallet.transfer(1000);
    }
}
```
