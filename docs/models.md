# Models
This section give you a rough idea of which function does what in a sample way.

# Vote Factory
Factory of vote sessions.


**Atribute**

| Type         | Name                | Definition                                |
|--------------|---------------------|-------------------------------------------|
| mapping      | voteSessions        | map keep voteID, vote session address     |
| uint         | voteId              | represent current vote session            |
| address      | owner               | represent admin                           |
| VoteExchange | voteExchange        | represent vote exchange’s record contract |



**Method**

| Name,Parameters      | Definition                      |
|----------------------|---------------------------------|
| createVoteSesion()   | create new vote session         |
| getLatestSession()   | get latest vote session address |



# Vote Session
A child of Vote Factory handle vote stuffs.



**UPDATE in Iteration2**

- add array that keep twitterID of all candidate.
- add method that take twitterID as input and return amount of vote.
- in end vote add random number to find winner who get NFT


**Atribute**

| Type                   | Name                   | Definition                                   |
|------------------------|------------------------|----------------------------------------------|
| uint                   | voteID                 | // represent this voteID                     |
| mapping                | riceParticipants       | //map keep address, total used vote (int)    |
| mapping                | voteMap                | //map keep twitterID (str), total vote (int) |
| VoteExchange           | voteExchange           | //represent vote exchange’s record contract  |
| enum {STARTED, ENDED}  | status                 | //check current state                        |

**Method**

| Name,Parameters         | Definition                                      |
|-------------------------|-------------------------------------------------|
| vote(amount, twitterID) | check first if rice exceed amount in Exchange   |
| endVote()               | end vote when admin end it or time limit exceed |


# Vote Exchange  
Keep rice token and give user a right for votes.

**Atribute**

| Type                  | Name          | Definition                                  |
|-----------------------|---------------|---------------------------------------------|
| maoping               | voteExchange  | map address, total rice (int)               |
| enum {OPENED, CLOSED} | status        | keep status of exchange                     |
| RICE                  | token         | represent token that used for vote exchange |

**Method**

| Name,Parameters  | Definition                        |
|------------------|-----------------------------------|
| deposit(amount)  | add rice in if exchange is open   |
| withdraw(amount) | take rice out if exchange is open |
| closeExchange()  | change state to close             |
| openExchange()   | change state to open              |



# NFT
Mint NFT and send to winner.

after `endVote()` in VoteSessions let's set state of this contract to open and send winner address to this contract. After owner come and upload some image to IPSF and press mint button it should mint a NFT and send to winner address.

BEAWARE: owner must not be able to mint NFT if NFT contract is closed.
after NFT is mint it should go back to close. The way to open is only when `voteEnd()`. Only owner is allow to mint.


| Type                  | Name          | Definition                                  |
|-----------------------|---------------|---------------------------------------------|
| address               | winnerAddress | hold the address to sent NFT to.               |
| enum {OPENED, CLOSED} | status        | keep status of contract                     |

**Method**

| Name,Parameters  | Definition                        |
|------------------|-----------------------------------|
| mint()  | mint NFT send it to winnerAddress and set state back to closed.   |


# Stake  
Take pair of token to stake in Pool

**Atribute**

| Type                  | Name          | Definition                                  |
|-----------------------|---------------|---------------------------------------------|
| PoolFactory             | poolFactory | keep the pool factory contract              |


**Method**

| Name,Parameters  | Definition                        |
|------------------|-----------------------------------|
| stake(token0,token1,amount token0 amount token1)  | stake token0,token1 by finding exact amount that suit for pool at that time.   |
| unstake(token0,token1,percent)| take token from stake back follow to percentage you pass in |
| getStakeAmount(token0,token1)  | get total amount that you have stake from those pool of those tokens             |

# Swap
Take one kind of ERC20 token and return to other kind of ERC20 token

**Atribute**

| Type                  | Name          | Definition                                  |
|-----------------------|---------------|---------------------------------------------|
| PoolFactory             | poolFactory | keep the pool factory contract              |
| MoneyBall            | MONEYBALL| the contract that keep fees.              |


**Method**

| Name,Parameters  | Definition                        |
|------------------|-----------------------------------|
| swap(token0,token1,amount token0)  | find pool and begin swap calculation   |
| getTokenOdds(token0,token1,amount token0)| return amount of token1 you will got back|

# PoolFactory
Hold all pool address and allow to create them.

**Atribute**

| Type                  | Name          | Definition                                  |
|-----------------------|---------------|---------------------------------------------|
| mapping(mapping)           | pools | keep  address token0 lead to token1 that lead to pool address.            |



**Method**

| Name,Parameters  | Definition                        |
|------------------|-----------------------------------|
| createNewPool(token0,token1,amount token0,amount token1)  | create new pool with that tokens and put starter amount in the pool. |
| getPool(token0,token1)| return pool(type Pool) of input tokens|
| getPoolAddress(token0,token1)| return pool address of input tokens|
| getTotalAmountInPool(token0,token1)| return amount of token you have stake in this pool|

# Pool
All key to stake and swap is locate here.

**Atribute**

| Type                  | Name          | Definition                                  |
|-----------------------|---------------|---------------------------------------------|
| ERC20           | token0 | token0 of this pool|
| ERC20           | token1 | token1 of this pool|
| uint256          | token0amount | total amount of token0 in this pool|
| uint256           | token1amount | total amount of token1 in this pool|
| mapping           | walletToken0 | take staker address map to amount of token0 they stake |
| mapping           | walletToken1 | take staker address map to amount of token1 they stake|


**Method**

| Name,Parameters  | Definition                        |
|------------------|-----------------------------------|
| token1NeedForStake(amount token0)  | return amount of token1 need to stake given amount of token0 |
| token0NeedForStake(amount token1)  | return amount of token0 need to stake given amount of token1 |
| fillPool(amount token0,amount token1, guy)| put those amount of token in pool from (guy staker's address)|
| drainPool(percent, guy)| return those amount of token from pool to (guy staker's address)|
| swap0to1(amount token0, amount token1)| send the amount of token1 to swaper|
| swap1to0(amount token0, amount token1)| send the amount of token0 to swaper|
