Cosmos
========

* Tendermint is a PoS consensus mechanism.
* ABCI is an interface for how application-specific state will be replicated by a consensus algo.  Tendermint complies with ABCI.
* IBC is a protocol for how blockchains to communicate with each other
* The Cosmos SDK is a library with implementations of Tendermint and IBC, for use in building a new blockchain.
* The Cosmos Hub is a specific blockchain intended to serve as a master connector of many blockchains that use IBC

# Consensus Mechanism
* [Tendermint whitepaper ("The latest gossip on BFT consensus")](https://arxiv.org/pdf/1807.04938.pdf) (2018)
  * describing the Tendermint consensus algo
* [Tendermint vs BFT](https://blog.cosmos.network/tendermint-vs-pbft-12e9f294c9ab)
* [Jepsen: Tendermint](https://jepsen.io/analyses/tendermint-0-10-2)
  * real-world test of whether Tendermint actually satisfies its theoretical safety properties.

# Cosmos Network
* [Cosmos whitepaper](https://v1.cosmos.network/resources/whitepaper) (2016)
  * Cosmos _network_ whitepaper (describing Cosmos zones/IBC and validator behavior - a network built on Tendermint consensus algo).
    See "Consensus systems" for a good summary of Tendermint vs PBFT and other consensus algos.
* [Cosmos & Polkadot comparison](https://juliankoh.medium.com/5-differences-between-cosmos-polkadot-67f09535594b) (2019)

# Ecosystem
* [The Cosmos ecosystem has arrived](https://members.delphidigital.io/reports/the-cosmos-ecosystem-has-arrived/) (Sep 2021)
