Sui
===========

Sui is an L1 that achieves high scalability by using causal ordering of transactions and parallel agreement, which are in turn enabled by an object-centric ownership system. Sui objects keep careful track of information about their owners. Objects can be immutably shared in addition to being mutably shared—a unique feature of Sui. Observing that most transactions do not compete for shared resources, Sui uses Byzantine consistent broadcast to execute most transactions on parallel, falling back on BFT consensus for transactions involving shared objects. Sui validates transactions one at a time, instead of in blocks. Objects in Sui are also versioned; the causal history of an object can be reverse-engineered by a validator with access to state information.

In summary, Sui’s performance gains can be attributed to Amdahl’s law — Sui optimizes for the common use case (transactions involving owned objects can be parallelized), while giving up performance in the uncommon use case (transactions involving shared objects).

## Blockchain
* [Sui Whitepaper](https://github.com/MystenLabs/sui/blob/main/doc/paper/sui.pdf)
* [Sui Framework](https://github.com/MystenLabs/sui/tree/main/crates/sui-framework)

## Ecosystem
* [Sui Developer Hub](https://docs.sui.io/?utm_source=medium&utm_medium=blog&utm_campaign=suilaunch_medium&utm_id=suilaunch)
* [Sui Medium](https://medium.com/mysten-labs/announcing-sui-1f339fa0af08)
