Smart contract security
========================

# Education / best practices
* [Solcurity](https://github.com/transmissions11/solcurity) - opinions from [t11s](https://twitter.com/transmissions11) on security best practices
* [Eth smart contract security best practices](https://consensys.github.io/smart-contract-best-practices/)
  (see especially: [known attacks](https://consensys.github.io/smart-contract-best-practices/attacks/))
* [Smart Contract Weakness Registry](https://swcregistry.io/) - an enumeration of common weaknesses in smart contracts, with code samples
* [Most common smart contract bugs](https://medium.com/solidified/most-common-smart-contract-bugs-of-2020-c1edfe9340ac)

## Solana-specific
_See [SolanaProgramming/Security](SolanaProgramming.md#security)_


# Bughunting challenges:
* [Damn Vulnerable DeFi](https://www.damnvulnerabledefi.xyz/)
* [OpenZeppelin Ethernaut](https://ethernaut.openzeppelin.com/)
* [Capture the Ether](https://capturetheether.com/)
* [Paradigm CTF 2021](https://github.com/paradigm-operations/paradigm-ctf-2021)
* [Smart contract CTF list](https://github.com/PumpkingWok/CTFGym)

  
# Real-life vulnerabilities

## Summaries
* [Rekt leaderboard](https://rekt.news/leaderboard/)
* [SlowMist stats](https://hacked.slowmist.io/en/) - brief summaries of each hack
* [web3isgoinggreat.com](https://web3isgoinggreat.com/) - a (slanted) aggregator of recent hacks, rugpulls, and scams


## Notable issues and incidents, explained

This is only a sampling!  We'd recommend that smart contract devs review all major exploits
(the [rekt leaderboard](https://rekt.news/leaderboard/) is a great starting point) to learn 
from previous failures.

### Re-entrancy
Re-entrancy is a famous and common issue where the attacker can unexpectedly recursively call a function multiple times,
to get the contract's state variables into an unexpected state.

* For an overview, see [Preventing re-entrancy attacks: lessons from history](https://medium.com/amber-group/preventing-re-entrancy-attacks-lessons-from-history-c2d96480fac3)
* [Exploiting uniswap from reentrancy to actual profit](https://blog.openzeppelin.com/exploiting-uniswap-from-reentrancy-to-actual-profit/) (July 2019)
  * Clear explanation of reentrancy vulnerability on Uniswap introduced by ERC777.
* [Lendf.Me re-entrancy attack](https://valid.network/post/the-reentrancy-strikes-again-the-case-of-lendf-me) (May 2020)
  * ERC777 strikes again
* [Cream Finance AMP attack](https://medium.com/cream-finance/c-r-e-a-m-finance-post-mortem-amp-exploit-6ceb20a630c5) (Aug 2021)
  * ERC777 strikes again.  Cream Finance was susceptible, and AMP token had an ERC777-style contract
* [Grim Finance exploit](https://twitter.com/RugDocIO/status/1472293717725913089) (Dec 2021)
  * Grim Finance used a before-after pattern to determine how much was deposited (needed because
    some tokens have a transfer tax, in which case the deposited amount will differ from the transferred amount)
  * Grim didn't guard against reentrancy between the before-after pattern.
  * [More details](https://rekt.news/grim-finance-rekt/)
* TheDAO exploit of 2016: the original re-entrancy bug, but listed last here because it's one of the hardest to understand.
  * [Race-to-Empty](https://vessenes.com/more-ethereum-attacks-race-to-empty-is-the-real-deal/)
    (written just before the exploit, describing a generic vulnerability)
  * [Analysis of TheDAO exploit](https://hackingdistributed.com/2016/06/18/analysis-of-the-dao-exploit/)
    (detailed explanation of the flaw as applied to TheDAO)
  * For more historical background (less technical) see [a history of 'The DAO' hack](https://coinmarketcap.com/alexandria/article/a-history-of-the-dao-hack)

### Oracle attacks
Some AMMs provide on-chain oracle functions (i.e. to compute asset prices from the current state of their pools).
Unfortunately, this could allow an attacker to manipulate the state of a pool (especially using a flash loan),
then do something else on a different protocol which depends on that oracle price.  Developers of
protocols that depend on on-chain oracles for pricing should be especially cognizant of this.

* [Cream Finance 130M hack](https://medium.com/@AndyPavia/swissblock-post-mortem-cream-finance-hack-7c1caff4335c) (Oct 2021)
  * oracle attack on a lending protocol due to a flawed custom oracle for yearn assets
    (see also: [cream hack analysis](https://mudit.blog/cream-hack-analysis/))
* [PancakeBunny reward overmint](https://medium.com/amber-group/bsc-flash-loan-attack-pancakebunny-3361b6d814fd) (May 2021)
  * oracle manipulation attack on PancakeBunny AMM
  * attacker gets way too many BUNNY reward tokens for LPing by unstaking in the middle of a massive mispricing from a flashloan
* [Enzyme finance custom oracle bug](https://medium.com/immunefi/enzyme-finance-price-oracle-manipulation-bug-fix-postmortem-4e1f3d4201b5)
  * an issue showing an interesting interaction between a governance token's custom oracle
    and its support for flashloans
* [Visor finance pricing exploit](https://twitter.com/Mudit__Gupta/status/1464657484367339527) (Nov 2021)
  * reliance on spot prices for issuing shares
* [Rari pool attack - TWAP manipulation of VUSD](https://twitter.com/Mudit__Gupta/status/1455627465678749696)
  * a specific pool was seemingly misconfigured to point at a pool with only concentrated liquidity
  * thread includes discussion of how it is easier to manipulate a pool with only concentrated liquidity because trading
    loss is relatively small
  * discussion of how TWAPs are still vulnerable because single huge input can move average a lot
* [Harvest Finance exploit](https://twitter.com/valentinmihov/status/1320667338321154048?s=20) (Oct 2020)
  * exploiter moved USDT/USDC on Curve up before depositing USDT into Harvest Finance, then down before withdrawing
  * pool share calc uses market price as oracle instead of 1
* [Oracle vulnerabilities](https://samczsun.com/so-you-want-to-use-a-price-oracle/)
  * samczsun discussion of some famous oracle attacks
* [Inverse Finance oracle attack](https://rekt.news/inverse-finance-rekt/) (Apr 2022) (required price manipulation across multiple blocks)
* [Inverse Finance oracle attack 2](https://twitter.com/peckshield/status/1537382891230883841) (June 2022)
* Related: [discussion of UniV3 TWAP oracle manipulation](https://twitter.com/euler_mab/status/1459314402059034634)

### Other interesting economic attacks
* [bZx 2020 exploit](https://peckshield.medium.com/bzx-hack-full-disclosure-with-detailed-profit-analysis-e6b1fa9b18fc) (Feb 2020)
  * lending protocol bZx allowed fancier functionality than a typical lending protocol, specifically
    allowing a user to put on a leveraged equity/debt position by routing to an AMM
  * a missing check caused the protocol to be fooled into taking a negative-value position while moving an AMM price way out of line
  * attacker made money by arbing the AMM back into line outside of the lending protocol, while abandoning the negative-value vault
  * [another](https://www.palkeo.com/en/projets/ethereum/bzx.html) great description of this issue
  * [another](https://lev.liv.nev.org.uk/pub/bzx_debug.txt) great description
* [Spartan Protocol LP share value calc issue](https://medium.com/amber-group/exploiting-spartan-protocols-lp-share-calculation-flaws-391437855e74) (May 2021)
  * mechanical flaw in calculation of LP share value in a synthetic asset protocol

### Bridge attacks
Bridges are complex because they involve multiple chains, and interaction with a third party.
Also, from the perspective of a single chain, transfers to that chain just involve unlocking
tokens (or minting claim tokens) from the bridge contract.

* [Poly network hack](https://slowmist.medium.com/the-analysis-and-q-a-of-poly-network-being-hacked-8112a35beb39) (Aug 2021)
  * more commentary [here](https://mudit.blog/poly-network-largest-crypto-hack/)
* [Polygon PoS bridge withdrawal bug](https://medium.com/immunefi/polygon-double-spend-bug-fix-postmortem-2m-bounty-5a1db09db7f1) (Oct 2021)
  * bug allowing repeat withdrawals from the bridge contract

### Missing checks
* [Polygon lack of balance/allowance check in MRC20 contract](https://medium.com/immunefi/polygon-lack-of-balance-check-bugfix-postmortem-2-2m-bounty-64ec66c24c7d) (Dec 2021)

### Uninitialized proxy
The [proxy upgrade pattern](https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies) is a very popular mechanism for making code upgradeable by moving business logic to an implementation contract while maintaining state on a proxy contract.  The proxy contract maintains a (switchable) pointer to the implementation and delegatecalls to the implementation contract for business logic.
* Unfortunately, there at one point there was a vulnerability in the standard OpenZeppelin pattern where the proxy contract could be bricked (funds in the proxy could be stuck permanently) which was fixed [here](https://forum.openzeppelin.com/t/security-advisory-initialize-uups-implementation-contracts/15301).
* [Uninitialized proxy in Harvest finance (bounty)](https://medium.com/immunefi/harvest-finance-uninitialized-proxies-bug-fix-postmortem-ea5c0f7af96b) (Nov 2021)
* [Uninitialized proxy in Wormhole bridge contract (bounty)](https://medium.com/immunefi/wormhole-uninitialized-proxy-bugfix-review-90250c41a43a) (Feb 2022)

### Unauthorized access
* [Pickle Finance exploit](https://rekt.news/pickle-finance-rekt/)
  * See especially [this diagram](https://twitter.com/vasa_develop/status/1330532691205361664/photo/1)
  * Pickle Finance was a fork of Yearn; Yearn published this [vulnerability disclosure](https://github.com/yearn/yearn-security/blob/master/disclosures/2020-10-10.md) a month earlier
  * Similar problem [here](https://rekt.news/rari-capital-rekt/)
* [Parity wallet bug](https://hackingdistributed.com/2017/07/22/deep-dive-parity-bug/) (2017)

### Frontend attacks
* [BadgerDAO Cloudflare exploit](https://badger.com/technical-post-mortem) (Dec 2021)
  * frontend attack arising from Cloudflare bug which allowed attackers to preregister API keys by email address
    without email verification
  * attacker used access to inject malicious scripts that prompted users to authorize
    tokens via MetaMask.

### Logic bugs
Arguably all bugs are logic bugs, but some seem like pure logic issues...

* [Compound overdistribution of governance token](https://twitter.com/Mudit__Gupta/status/1443454935639609345?t=sznwoTmp3KMDffaI1vpINA&s=19) (Sep 2021)
  * (see also [this](https://cybavo.medium.com/defi-protocol-compound-suffers-a-90m-reverse-rugpull-after-1-letter-bug-6168071497e2))
* [Popsicle Finance exploit](https://twitter.com/Mudit__Gupta/status/1422797923037814786?s=20)
  * bug in computing users' share of fees when LP shares are transferred
  * notable in that bug had been repeatedly exploited in other contracts, but was missed by creators and auditors
* [MonoX hack](https://twitter.com/Mudit__Gupta/status/1465726874974187524) (Nov 2021)
  * vAMM protocol for trading synthetics
  * when user swaps A for B, vAMM updates price of A to be lower than before, then updates price of B to be higher than before
  * MonoX didn't prevent corner case where A == B, so user could use this to increase price of B
  * attacker used this repeatedly to pump internal price of MONO token, then swap MONO into a lot of real value
* [Opyn bug](https://peckshield.medium.com/opyn-hacks-root-cause-analysis-c65f3fe249db) (Aug 2020)
  * bug stemming from special case for ETH transfers

## Other
* [Interview with whitehat samczsun](https://medium.com/immunefi/the-u-up-files-with-samczsun-1a9116cf6e74)
