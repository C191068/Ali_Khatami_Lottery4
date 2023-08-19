# Ali_Khatami_Lottery4(Learning from the video of Patrick Collins)

### Implementing the Chainlink VRF(The Fulfill)

### Modulo



![c37](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/5c743bdb-4d2c-4561-bb90-0ab8b28bcde3)

once we get the random number we gonna pick random winner from the arrays of participants from here <br>


Lets pick a random winner using something called module function <br>

We gonna get array back of random numbers if we will <br>

![c38](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/f583ed02-b052-4268-a355-ee239a978116)

since we are requesting one random word this random words array can be of size 1 <br>

![c39](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/1657a2ff-1db9-41ef-b682-d7b1e4e17743)


we can learn about modulo function by going to this link https://docs.soliditylang.org/en/v0.8.17/types.html

we use module function to pick a random number out of our player array <br>


Let our palyer array size is 10 and random number is 202 <br>

So how do we pick a random person out of this player array <br>


![c40](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/952615d8-029c-4421-baca-42261d14ea32)

 for that we will do 202 mod 10 = 2


here we gonna get result from 0 to 9 which works out perfectly because those are indexes of 10 people <br>


![c41](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/a446c09b-3f85-4786-9518-4581c8790abc)

In code we use it in the above way where index is zero because we will generate here one random number <br>

```uint256 indexofChampion``` will give us the index of our random winner <br>

![c42](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/8093814f-2f21-4567-ba96-c3b78b7920aa)


To get the address of the winner we write the above code <br>

Thus we can get address of the person thats gonna be verifiably random winner <br>



![c43](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/305e1557-55f7-4711-814a-a0fe67aaaeb2)

Now we gonna create a state variable for our recent Champion at first it will start of nobody  <br>

![c44](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/d374e053-f79a-4f66-95f9-e5b613f05797)


It will get updated as we use the above keyword <br>

![c45](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/e6ffa457-d0ad-4418-958e-d217017bef59)

we want people to know who the recent winner is and thus we use the above code  <br>

s_recentChampion; gonna be storage variable <br>

Since we got our recent winner we now gonna send them money in this contract <br>

![c46](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/473a2b50-537e-4599-ade0-02c8f3c5b3da)

For that we use the line of code <br>


![c47](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/2bdd3e47-77ea-4b43-ab58-65f537c702ab)


To revert error message we use the above line of code <br>

![c48](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/2cba834b-0179-4dd2-9f96-f6a1dbe1bd14)


Then we use the above line of code <br>



```

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";
import "@chainlink/contracts/src/v0.8/interfaces/VRFCoordinatorV2Interface.sol";

error akrkLottery_NotEnoughEthEntered();
error akrkLottery_TransferFailed();

contract akrkLottery is VRFConsumerBaseV2 {
    //below we gonna pick minimum price and it gonna be storage variable
    //visibiliy will be private but it will be configurable
    //We will cover our both storage and non storage variables under state variables section

    /* State variables */

    uint256 private immutable i_welcomeFee;
    //We probably also nedd to track of all the users who entered the lottery
    //participants is a storage variable because we gonna modify this a lot
    address payable[] private s_participants;

    VRFCoordinatorV2Interface private immutable i_vrfCoordinator;

    bytes32 private immutable i_gasLane;

    uint64 private immutable i_subscriptionId;

    uint32 private immutable i_callbackGaslimit;

    uint16 private constant REQUEST_CONFIRMATIONS = 3;

    uint32 private constant NUM_WORDS = 1;

    address private s_recentChampion;

    /* Events */

    event LotteryEnter(address indexed participants);

    event RequestedLotteryChampion(uint256 indexed requestId);

    // to configure it we st constructor below

    constructor(
        address vrfCoordinatorV2,
        uint256 welcomeFee,
        bytes32 gasLane,
        uint64 subscriptionId,
        uint32 callbackGaslimit
    ) VRFConsumerBaseV2(vrfCoordinatorV2) {
        i_welcomeFee = welcomeFee;

        i_vrfCoordinator = VRFCoordinatorV2Interface(vrfCoordinatorV2);

        i_gasLane = gasLane;

        i_subscriptionId = subscriptionId;

        i_callbackGaslimit = callbackGasLimit;
    }

    //to enter the lottery we created a function below

    function enterLottery() public payable {
        if (msg.value < i_welcomeFee) {
            revert akrkLottery_NotEnoughEthEntered();
        }

        s_participants.push(payable(msg.sender));

        emit LotteryEnter(msg.sender);
    }

    //to pick a random champion we created the function below
    //The below function is gonna be called by chainlink keepers network

    function requestRandomchampion() external {
        uint256 requestId = i_vrfCoordinator.requestRandomWords(
            i_gasLane,
            i_subscriptionId,
            REQUEST_CONFIRMATIONS,
            i_callbackGaslimit,
            NUM_WORDS
        );

        emit RequestedLotteryChampion(requestId);
    }

    function fulfillRandomWords(uint256 requestId, uint256[] memory randomWords) internal override {
        uint256 indexofChampion = randomWords[0] % s_participants.length;

        address payable recentChampion = s_participants[indexofChampion];

        s_recentChampion = recentChampion;

        (bool success, ) = recentChampion.call{value: address(this).balance}("");

        if (!success) {
            revert akrkLottery_TransferFailed();
        }
    }

    //we want other users to see entrance fee so we created the function below
    /*View/Pure Function*/
    function getEntranceFee() public view returns (uint256) {
        return i_welcomeFee;
    }

    //to know who are in the participants array the function is created below
    function getParticipant(uint256 index) public view returns (address) {
        return s_participants[index];
    }

    function getRecentChampion() public view returns (address) {
        return s_recentChampion;
    }
}

```


Now we don't have a way to keep track of the list of previous winners <br>

So we gonna emit an event <br>



so there is gonna be easily query able history of event winners <br>



![c49](https://github.com/C191068/Ali_Khatami_Lottery4/assets/89090776/9dda881a-a087-4c0a-863a-7b335104ad45)

So we gonnna create a new event in the event section <br>



















