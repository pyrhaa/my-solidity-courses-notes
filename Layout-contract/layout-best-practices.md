En se référant à la doc officielle de [Solidity](https://docs.soliditylang.org/en/latest/layout-of-source-files.html), la meilleur manière d'organiser son layout contract est ci-dessous.

#### Layout contract elements order

1. pragma
2. imports
3. interfaces
4. librairies
5. contrats

##### Dans chaque contrat, librairy ou interface

1. state variables
2. events
3. fonction modifiers
4. struct, arrays ou enums
5. constructor
6. fallback - receive fonction
7. external visible fonctions
8. public visible fonctions
9. internal visible fonctions
10. private visible fonctions

Voici un exemple ci-dessous, lisez les comments et examinez les exemples. Concentrez-vous sur le layout, pas sur la logique du code ici.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

/// @title Layout of a contract
contract Layout {
    // state variables
    address public admin;
    string public titleOfLayout;
    uint public price;
    bool public isActive = false;

    // events
    event priceChanged(uint previousPrice, uint currentPrice);
    event titleIsSet (address setter, uint timestamp);

    // function modifiers
    modifier onlyAdmin {
        require(msg.sender == admin, "Only Admin!");
        _;
    }

    // struct, arrays or enums
    enum State { PENDING, RESOLVED, REJECTED }
    struct User {
        string fullName;
        address wallet;
        uint phone;
    }
    mapping(address => uint) balance;

    // constructor, initialize state variables within constructor
    constructor(address _admin, uint _price) {
        admin = _admin;
        price = _price;
    }

    // receive - fallback function if exist
    // receive ether function
    receive() external payable {}

    // external functions
    function setTitle(string calldata title) external onlyAdmin() {
        titleOfLayout = title;
        emit titleIsSet(admin, block.timestamp);
    }

    // public functions
    function addition(uint a, uint b) public pure returns(uint) {
        return a + b;
    }

    //internal functions
    function internalMesage() internal pure returns(string memory) {
        return "Internal message";
    }

    // private functions
    function makeActive() private {
        isActive = true;
    }
}
```
