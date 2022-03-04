DeFi
=====

## Contents
* [Overview](#overview)
* [Lending Protocols](#lending-protocols)
* [Dex trading](#dex-trading)
  * [AMMs](#amm-swap-according-to-a-formula-dexes)
  * [Serum](#serum---full-order-book)
  * [Derivatives](#derivatives)
* [Stablecoins](#stablecoins)
* [Bridges](#bridges)
* [Oracles](#oracles)
* [Indexing](#indexing)
* [Insurance](#insurance)
* [Governance](#governance-wars-and-defi)
* [Tools](#tools)

## Overview
* [Finematics guide to DeFi](https://finematics.com/guide-to-decentralized-finance/)
  * a skill tree of DeFi concepts
* [Where does yield come from, anyway?](https://juliankoh.medium.com/where-does-yield-come-from-anyway-fc818c114bd5)
* [A theory of DeFi?](https://www.youtube.com/watch?v=esekWF3SyB8)
  * an overview of DeFi and why it's important develop "theory". Deep-dives into the AMM primitive

## Lending protocols
* [Lending and borrowing in DeFi](https://finematics.com/lending-and-borrowing-in-defi-explained/) (Finematics, Nov 2020)
* [Flash loans](https://hackingdistributed.com/2020/03/11/flash-loans/)
  * explaining the flash loan concept pioneered by Aave, as well as how it's used in various arbitrage scenarios
* Optional source material:
  * [Aave whitepaper](https://github.com/aave/aave-protocol/blob/master/docs/Aave_Protocol_Whitepaper_v1_0.pdf) (Dec 2020)
  * [Compound whitepaper](https://compound.finance/documents/Compound.Whitepaper.pdf) (Feb 2019)

## DEX trading
### AMM (swap according to a formula) DEXes
* [Constant Function Market-Makers](https://arxiv.org/pdf/2003.10001.pdf) (June 2020)
  * a theoretical assessment of the more general version of constant-product market-makers
* [Perp AMMs](https://insights.deribit.com/market-research/the-quest-for-perp-amms/)
  * explaining the vAMM method introduced by Perpetual Protocol and its current pitfalls
* [Explaining balancer pools](https://medium.com/balancer-simulations/understanding-balancer-pools-c2b877dcc082)
* [UniV3 Simulator](https://defi-lab.xyz/uniswapv3simulator)

### Serum (full order book)
* [Serum whitepaper](https://assets.website-files.com/61382d4555f82a75dc677b6f/61384a6d5c937269dbed185c_serum_white_paper.88d98f84.pdf)
* [A technical introduction to the Serum DEX](https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit)
  * describes the orderbook interactions and data structures, as well as how they're implemented. Note the section on the request queue is outdated
* [Serum core](https://docs.projectserum.com/appendix/serum-core)
  * a next-gen orderbook that decouples the matching engine from SPL tokens. Also known as the agnostic orderbook (AOB)
  * [DEX v4](https://github.com/Bonfida/dex-v4) is a new version of Serum DEX that builds on top of the AOB

### Derivatives
* [DeFi Derivative Landscape](https://github.com/0xperp/defi-derivatives)

## Stablecoins
* [Stablecoin Overview](https://www.youtube.com/watch?v=0XB_2O6FsIk)
  * a general overview of the stablecoin landscape and key differentiators for categorization
* [MakerDao Whitepaper](https://makerdao.com/en/whitepaper/#abstract)
  * the original crypto-collateralized stablecoin
* [Terra Whitepaper](https://assets.website-files.com/611153e7af981472d8da199c/618b02d13e938ae1f8ad1e45_Terra_White_paper.pdf)
* [Black Thursday for MakerDao, An Explainer](https://medium.com/@whiterabbit_hq/black-thursday-for-makerdao-8-32-million-was-liquidated-for-0-dai-36b83cac56b6)
  * a general overview of MakerDao's Black Thursday event
* [Deleverging Spirals and Stablecoins Attacks](https://arxiv.org/pdf/1906.02152.pdf)
  * a deep dive into deleveraging spirals, mempool attacks and an empirical investigation of MakerDao's Black Thursday event
* [The Iron Finance Exploit Explained](https://thedefiant.io/not-just-a-bank-run-new-evidence-shows-iron-finance-crashed-due-to-code-exploit/)

## Bridges
* [Bridge survey](https://medium.com/1kxnetwork/blockchain-bridges-5db6afac44f8)

## Oracles
* [The Oracle Report of Delphi](https://members.delphidigital.io/reports/the-oracle-report-of-delphi/) (June 2021)
  * overview of the oracle landscape

## Indexing
* [Intro to The Graph](https://thegraph.com/docs/about/introduction)
  * The Graph is a decentralized protocol for indexing and querying blockchain data
    * It's like a search engine framework, and each 'subgraph' is a domain-specific search engine implementation
  * [GraphQL API](https://thegraph.com/docs/developer/graphql-api)
    * tutorial on how to query a subgraph using GraphQL

## Insurance
* [Nexus Mutual (NXM) whitepaper](https://nexusmutual.io/assets/docs/nmx_white_paperv2_3.pdf)

## Governance wars and DeFi
* [CRV/CVX part 1](https://tokenbrice.xyz/defi-flywheel/) (June 2021)
  * describing the innovation of Convex (CVX) as a method for controlling valuable CRV governance votes,
    as well as the more general question of protocol design with respect to 'meta-protocols' like CVX
* [CRV/CVX part 2](https://tokenbrice.xyz/crv-wars/) (Sep 2021)
  * an in-depth look at the CRV governance war which TokenBrice correctly predicted in part 1
* [Mochi scam and the Curve Wars](https://www.coindesk.com/business/2021/11/11/curve-wars-heat-up-emergency-dao-invoked-after-clear-governance-attack/) (Nov 2021)
* [Convex - A Curve Overlay](https://www.tinkeringsociety.xyz/post/analysis-of-convex-finance-cvx) (Feb 2022)
  * a comprehensive analysis of Convex, its tokenomics, and its intimate relationship with Curve and Yearn
* [Tokemak - A Deep Dive](https://www.tinkeringsociety.xyz/post/tokemak-a-deep-dive)
  * a look at how Tokemak works and its vision of directed liquidity

## Tools
### High-level TVL/usage stats
* [Defi Llama](https://defillama.com/) - TVL metrics for various DeFi protocols
* [Defi pulse](https://defipulse.com/) - similar, TVL metrics
* [DappRadar](https://dappradar.com/)
* [Crypto fees](https://cryptofees.info/) - fees earned by protocol

### Portfolio
Use these to track your (or anyone else's) balances/activity across various DeFi protocols.
* [Zapper.fi](http://zapper.fi) - Eth
* [DeBank](https://debank.com/) - Eth
* [Sonar.watch](http://sonar.watch) - Solana
* [Step.finance](http://step.finance) - Solana
* [APY.vision](https://apy.vision/) - portfolio tracking / IL analysis / pool analytics

### Yield Farming Analytics
* [Multifarm.fi](https://alpha.multifarm.fi/farms) - overview of farms on various chains
* [LiquidityFolio](https://www.liquidityfolio.com/) - farm finder
* [DeFi Rate](https://defirate.com/) - comparison of lending/borrowing rates across lending protocols

### Smart Contract Security
* [apesafe.io](http://apesafe.io) - contract comparison
* [app.unrekt.net](http://app.unrekt.net) - allowance checker
* [RugDoc](http://rugdoc.io) - AMM emergency withdrawal tool, honeypot checker, LP breaker, etc
* [defiyield.info](https://defiyield.info/) - tools for reviewing where you approved ERC20 tokens, timelocks, etc

### Airdrops
* [twitter.com/defi_airdrops](https://twitter.com/defi_airdrops)

### Fun
* [fees.wtf](http://fees.wtf) - check how much you have spent on ETH fees
* [il.wtf](https://il.wtf) - check your cumulative IL
