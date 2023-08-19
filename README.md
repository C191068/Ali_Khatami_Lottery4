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

























