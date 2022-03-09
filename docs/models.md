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
