## Address types

#### Address

The address type comes in two flavours, which are largely identical:

- `address`: est un type qui correspond à une adresse sur Ethereum. Cette adresse peut être un account, ou celle d'un smart contract.

- `address payable`: Pareil qu' `address`, mais avec les possibilités `transfer` et `send`.

L'idée derrière cette distinction est que `address payable` est une addresse vers laquelle on peut envoyer de l'Ether,
tandis que `address` ne peut pas en recevoir.

#### Globales variables

- `msg.sender`: est une fonction intégrée([global variables](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#address-related)) à Solidity qui indique l'adresse Ethereum en cours de connexion avec le contrat. Son type est `address payable`.
