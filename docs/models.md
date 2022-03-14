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

- add 3 atribute to keep first twitterID that got most votes and so on.
- add method to grab those 3 attribute
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
