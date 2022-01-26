Avalanche
===========

Avalanche defines a consensus mechanism called Avalanche Consensus, which replaces the linear blockchain with a DAG
and uses random subsampling for conflict resolution within conflict sets.

Snowman Consensus is the linear version of Avalanche Consensus.  Validator coordination and smart contract execution in Avalanche use this because they require total ordering.

There are at least 3 chains in Avalanche:
* Exchange (X) Chain: uses Avalanche Consensus; used for creating and exchanging assets.  Assets are tracked using the UTXO model in order to leverage the speedup from the DAG.
* Platform (P) Chain: uses Snowman Consensus; used for validator consensus and definition of other subnets
* Contract (C) Chain: uses Snowman Consensus; runs smart contracts (including those written for EVM).


# Blockchain
* [Avalanche consensus](https://docs.avax.network/learn/platform-overview/avalanche-consensus/)
  * [Avalanche consensus whitepaper](https://assets.website-files.com/5d80307810123f5ffbb34d6e/6009805681b416f34dcae012_Avalanche%20Consensus%20Whitepaper.pdf) (Aug 2020)
* [Avalanche platform whitepaper](https://assets.website-files.com/5d80307810123f5ffbb34d6e/6008d7bbf8b10d1eb01e7e16_Avalanche%20Platform%20Whitepaper.pdf) (June 2020)


# Ecosystem
* [Avalanche skill tree](https://twitter.com/Darrenlautf/status/1428565212605546499) (Darren Lau)
* [Avalanche ecosystem overview](https://research.thetie.io/avalanche-ecosystem/) (The Tie, July 2021)
