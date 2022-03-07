# Models
This section give you a rough idea of which function does what in a sample way.

# Vote Factory
Act as a parent of vote sessions.


**Atribute**

| Type    | Name                | Definition                                |
|---------|---------------------|-------------------------------------------|
| Mapping | voteSessions        | map keep voteID, vote session address     |
| Uint256 | voteID              | represent current vote session               |
| Address | owner               | represent admin                           |
| Address | voteExchangeAddress | represent vote exchange’s record contract |



**Method**

| Name,Parameters                           | Definition          |
|-------------------------------------------|--------------------------|
| createVoteSesion(voteID,admin,SFVaddress) | create new vote session  |
| getVoteSession(index)                     | get vote session address |



# Vote Session
A child of Vote Factory handle vote stuffs.

**Atribute**

| Type    | Name                   | Definition                                   |
|---------|------------------------|----------------------------------------------|
| Int     | voteID                 | // represent this voteID                     |
| Mapping | riceParticipants       | //map keep address, total rice (int)         |
| Mapping | voteMap                | //map keep twitterID (str), total vote (int) |
| Address | voteExchangeAddress    | //represent vote exchange’s record contract  |
| Enum    | voteState= {start,end} | //check currently state                      |

**Method**

| Name,Parameters         | Definition                                      |
|-------------------------|-------------------------------------------------|
| vote(amount, twitterID) | check first if rice exceed amount in Exchange |
| end_vote()              | only admin or time exceed                       |


# Vote Exchange  
Keep rice token and give user a right for votes.

**Atribute**

| Type    | Name          | Definition                |
|---------|---------------|---------------------------|
| Mapping | voteExchange  | //map address, total rice |
| Enum    | exchangeState | keep status of exchange   |

**Method**

| Name,Parameters  | Definition                       |
|------------------|----------------------------------|
| deposit(amount)  | add rice in if stake is open   |
| withdraw(amount) | take rice out if stake is open |
| closeExchange()  | change state to close          |
| openExchange()   | change state to open           |
