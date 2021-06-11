Je voulais vous rappeler quelques points sur l'utilisation des structs. Voyons l'exemple ci-dessous ;

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.9.0;

contract StructUsages {

    // just a reference to previous article: Reference Types
    // struct A can contain struct B but struct A can’t contain its own type struct A.
    struct Payment {
        uint amount;
        uint timestamp;
        uint paymentId;
    }

    // works
    struct PaymentPlan {
        address sender;
        address payable receiver;
        Payment payment;
    }

    // doesn't work, recursive struct definition
    struct IllegalUsage {
        address sender;
        address payable receiver;
        Payment p;
        IllegalUsage i;
    }
}
```

Dernières notes sur les reference types. Si nous avons affaire à des reference types, nous devons être très prudents pour les raisons ci-dessous.

- Les reference types ne correspondent pas nécessairement à 32bytes — 256 bits.

- La quantité de gaz consommée pendant l'exécution dépend de la data location.
  En créeant les copies indépendantes depuis les references types sont chers, il est donc recommandé que la plupart des internal functions soient choisies pour travailler avec la memory data location.

- Nous devons être prudents dans le scénario où lorsque deux ou plusieurs variables différentes pointent vers la même data location puisque tout changement dans une variable aura un impact sur les autres.

---

Enfin, examinons une utilisation un peu compliquée mais très précieuse des structs, des mapping et des enums dans un <em>escrow</em> comme un contrat.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract EscrowContract {
    // state variables also we can call them storage variables
    address public lawyer;
    mapping(bytes32 => Escrow) public plans;

    // enum for state control
    enum State { INACTIVE, PENDING, ACTIVE, CLOSED }

    struct Escrow {
        string planName;
        address payable payer;
        address payable receiver;
        uint requiredAmount;
        bytes32 escrowId;
        State state;
    }

    modifier onlyLawyer() {
        require(msg.sender == lawyer, 'Only lawyer!');
        _;
    }

    modifier inState(bytes32 _escrowId, State state) {
        require(plans[_escrowId].state == state, 'Invalid State!');
        _;
    }

    // after deployment of contract, msg.sender will be lawyer
    constructor() {
        lawyer = msg.sender;
    }

    /**
    * @notice lawyer generate random id for attaching it
    * to it's escrow plan
    *
    * @dev Warning: it isn’t an easy task to generate true random input.
    * Do not rely on block.timestamp as a source of randomness.
    * Since this value can be manipulated by miners.
    *
    */
    function idGenerator(
        address payable _payer,
        address payable _receiver,
        uint _requiredAmount
    )   external
        view
        onlyLawyer()
        returns (bytes32)
    {
        return keccak256(
            abi.encode(
                _payer,
                _receiver,
                _requiredAmount,
                block.timestamp
            )
        );
    }

    /**
    * @notice adding new escrow plan with an id
    *
    * remember id is generated via above function by lawyer
    *
    */
    function addEscrowPlan(
        string memory _planName,
        address payable _payer,
        address payable _receiver,
        uint _requiredAmount,
        bytes32 _escrowId

    )
        external
        onlyLawyer()
        inState(_escrowId, State.INACTIVE)
    {
        plans[_escrowId] = Escrow(
             _planName,
            _payer,
            _receiver,
            _requiredAmount,
            _escrowId,
            State.PENDING
            );
    }

    /**
    * @notice deposit Ether functionality, only payer can execute this function
    *
    * @param _escrowId required plan id for deposit ether
    */
    function depositEther(bytes32 _escrowId)
        external
        payable
        inState(_escrowId, State.PENDING)
    {
        require(msg.sender == plans[_escrowId].payer, "Only payer!");
        require(
            msg.value >= plans[_escrowId].requiredAmount,
            'Invalid amount provided!'
        );

        if (msg.value > plans[_escrowId].requiredAmount) {
            plans[_escrowId].payer.transfer(
                msg.value - plans[_escrowId].requiredAmount
            );

            plans[_escrowId].state = State.ACTIVE;
        }

        plans[_escrowId].state = State.ACTIVE;
    }

    /**
    * @notice withdraw Ether functionality, only lawyer can execute this function
    *
    * @param _escrowId required plan id for withdraw ether
    */
    function withdrawEther(bytes32 _escrowId)
        external
        onlyLawyer()
        inState(_escrowId, State.ACTIVE)
    {
        plans[_escrowId].state = State.CLOSED;
        plans[_escrowId].receiver.transfer(plans[_escrowId].requiredAmount);
    }

    /**
    * @return contract balance
    */
    function contractBalance() external view returns (uint) {
        return address(this).balance;
    }
}
```
