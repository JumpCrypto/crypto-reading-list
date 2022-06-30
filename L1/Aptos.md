Aptos
===========

Aptos is an L1 that emphasizes safety and scalability. The Aptos VM is written in Rust, while its smart contract logic is based on the Move language. 
Move represents digital assets as programmable resources: data that can be moved from account to account, but not reused, duplicated, or discarded. Developers can publish [Move Modules](https://aptos.dev/tutorials/your-first-move-module) for handling assets to the Aptos blockchain. Aptos has both FullNodes (read-only) and Validator Nodes, both of which have a RESTful API. 

Aptos uses a version of Hotstuff BFT consensus, has an advertised TPS of >100k, and allows for parallel account transactions (via conflict-resistant sequence numbers). The Aptos Framework defines a standard library of Move modules accessible on-chain, which defines data structures, coins, ACLs, non-fungible tokens, and more. One interesting use case of the Move Signer type is multi-agent transactions, which are atomic transactions involving multiple signers.


## Blockchain
* [Aptos Framework](https://github.com/aptos-labs/aptos-core/tree/main/aptos-move/framework)
* [Aptos Core](https://github.com/aptos-labs/aptos-core)
* [Move Whitepaper](https://diem-developers-components.netlify.app/papers/diem-move-a-language-with-programmable-resources/2020-05-26.pdf)
* [The Move Book](https://move-book.com/index.html)


## Ecosystem
* [Aptos Developer Network](https://aptos.dev/)
* [Pontem Developer Network](https://pontem.network/)
* [Aptos Medium Blog](https://medium.com/aptoslabs/the-aptos-vision-1028ac56676e)
