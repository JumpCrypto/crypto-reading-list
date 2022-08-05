Sui
===========

Sui is an L1 that achieves high scalability by using causal ordering of transactions and parallel agreement, which are in turn enabled by an object-centric ownership system. Observing that most transactions do not compete for shared resources and involve only singly-owned objects, Sui uses Byzantine Consistent Broadcast to execute most transactions in parallel (see [FastPay](https://arxiv.org/pdf/2003.11506.pdf)), falling back on [BFT consensus (Narwhal-Tusk)](https://arxiv.org/pdf/2105.11827.pdf) for transactions involving shared objects.

It is possible to discern between these two cases because Sui objects keep careful track of information about their owners. As a consequence of the ownership system, Sui validates transactions one at a time, instead of in blocks, objects in Sui are versioned (the causal history of an object can be reverse-engineered by a validator with access to state information), and objects can be immutably shared in addition to being mutably shared, a unique feature of Sui.

In summary, Sui’s performance gains can be attributed to Amdahl’s law — Sui optimizes for the common use case (transactions involving owned objects can be parallelized), while giving up performance in the uncommon use case (transactions involving shared objects). Sui extends and borrows from the Move programming language to achieve a safe and informative object ownership system.

## Blockchain
* [Sui Whitepaper](https://github.com/MystenLabs/sui/blob/main/doc/paper/sui.pdf)
* [Sui objects dev tutorial](https://docs.sui.io/devnet/build/programming-with-objects)
* [Sui Framework codebase](https://github.com/MystenLabs/sui/tree/main/crates/sui-framework)
* [Move Book (by the Diem devs)](https://diem.github.io/move/introduction.html)
* [Move Book (by Damir, the Move Guy)](https://move-book.com/)
* [Narwhal-Tusk](https://arxiv.org/pdf/2105.11827.pdf)
* [FastPay](https://arxiv.org/pdf/2003.11506.pdf)

## Ecosystem
* [Sui Developer Hub](https://docs.sui.io/?utm_source=medium&utm_medium=blog&utm_campaign=suilaunch_medium&utm_id=suilaunch)
* [Sui Medium](https://medium.com/mysten-labs/announcing-sui-1f339fa0af08)
