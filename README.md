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

























