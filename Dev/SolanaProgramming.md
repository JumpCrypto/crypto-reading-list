Solana programming
===================

## Blockchain core concepts
_See [L1/Solana.md](../L1/Solana.md)_
* [Solana docs](https://docs.solana.com/introduction)

## SolDev.app
* [SolDev.app](https://www.soldev.app/) - excellent collection of resources, getting better by the day

## Tutorials
* [Solana Programming (the "escrow tutorial")](https://paulx.dev/blog/2021/01/14/programming-on-solana-an-introduction/)
  * the canonical tutorial on Solana programming by building up an escrow contract.  Warning: somewhat out of date;
    does not use anchor at all
* [Complete guide to full stack Solana development](https://dev.to/dabit3/the-complete-guide-to-full-stack-solana-development-with-react-anchor-rust-and-phantom-3291)
* [Create a Solana dApp from scratch](https://lorisleiva.com/create-a-solana-dapp-from-scratch)
* [Rust programming on Solana](https://brson.github.io/2021/06/08/rust-on-solana)
  * thoughtful opinions and useful tips by a Solana beginner who's very experienced in Rust (core developer)
* [SolDev.app library of walkthroughs](https://www.soldev.app/library/walkthroughs)
  * We haven't validated all of the pages linked from here, but a large (and growing)
    collection of links to helpful tutorials.
* [thread from redacted_noah](https://twitter.com/redacted_noah/status/1475208069969756166) on the solana dev skill tree
* [thread from ayushmenon_](https://twitter.com/ayushmenon_/status/1476294409205526534)

## Learning Rust

### Essential
* [Rust Lang book ("The Book")](https://doc.rust-lang.org/book/)
  * written by Rust's own documentation team. A must-read for newcomers.
  * accompanying CLI-based [exercises](https://github.com/rust-lang/rustlings)
* [Exercism rust exercises](https://exercism.org/tracks/rust)

### Bonus
* [Rust by Example](https://doc.rust-lang.org/rust-by-example/index.html)
* [The Rustonomicon ("Nomicon")](https://doc.rust-lang.org/nomicon)
  * the dark arts of unsafe Rust: educational even if you plan to never use `unsafe`.
* [Learn Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/)
  * these exercises involving linked lists require you to think deeply about Rust's memory model.
* [Jon Gjengset's youtube videos](https://www.youtube.com/c/JonGjengset)
  * educational streams showing how to apply Rust to real-world problems.
  * [Rust for Rustaceans](https://nostarch.com/rust-rustaceans) is a just-released book by Jon. Disclaimer: requires purchase (no affiliation).

## Reference
* [Solana docs](https://docs.solana.com/introduction)
* [Solana cookbook](https://solanacookbook.com/)
* [Anchor](https://github.com/project-serum/anchor)
  * framework for reducing boilerplate by defining a IDL connecting your rust program to your typescript code.
    See also the [tutorial](https://github.com/project-serum/anchor/tree/master/examples/tutorial).
    * [angkor wat](https://2501babe.github.io/posts/anchor101.html) - another post from 2501babe.github.io, this time about anchor programming
* [solana-program-library](https://github.com/solana-labs/solana-program-library)
  * a collection of examples and reference implementations
* [Metaplex](https://github.com/metaplex-foundation/metaplex)
  * defines the NFT standard on Solana; also standardized the minting process (candy machine).
  See also [community docs](https://docs.metaplex.com/community).

## Security
_See also: [Security.md](Security.md)_
* [Solana common pitfalls](https://blog.neodyme.io/posts/solana_common_pitfalls) (Neodyme, Aug 2021)
* [Solana Security Workshop](https://workshop.neodyme.io/) (Neodyme, Nov 2021)
* [Auditing Solana smart contracts - Part 1 (checklist)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-1-a-systematic-approach-56a434f6c9ed) (Nov 2021)
  * [Part 2 (autoscanning)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-2-automated-scanning-ceb88830ae6d)
  * [Part 3 (penetration testing)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-3-penetration-testing-a315b3bbb2d3)
  * [Part 4 (anchor)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-4-the-anchor-framework-ef42d944f086)
* [spl token-lending protocol exploit](https://blog.neodyme.io/posts/lending_disclosure) -
  walkthrough of an exploit in spl token-lending protocol identified by Neodyme, as an example of a flaw in a smart contract
* [Solend Auditing Workshop](https://docs.google.com/presentation/d/1jZ9kVo6hnhBsz3D2sywqpMojqLE5VTZtaXna7OHL1Uk/edit?pli=1#slide=id.ge15c343642_0_51)

## Devtools
* https://www.sollet.io/
* https://www.spl-token-ui.com/#/
* Essential CLI tools:
  * `solana`
  * `spl-token`
  * `anchor`

## Walkthrough
*Please feel free to directly edit: fix inaccuracies, expand content, condense sections, etc.*

Goal of this section: condense the above resources and compile useful tidbits (tips, gotchas) to reference in the future.

Let's also take a look at the [Serum](https://github.com/project-serum/serum-dex) code itself.
The escrow tutorial is excellent, but it would be interesting to look at a directly relevant real-world program.

- [Overview](#overview)
- [Anchor and Escrow](#anchor-and-escrow)
- [Walkthrough: Anchor and Escrow (WIP)](#walkthrough-anchor-and-escrow-wip)
- [Walkthrough: Serum (WIP)](#walkthrough-serum-wip)
- [APIs and the bigger picture](#apis-and-the-bigger-picture)
- [API evolution in Serum (WIP)](#api-evolution-in-serum-wip)
- [Accounts](#accounts-wip)
- [Highlights](#highlights)
- [Gotchas](#gotchas)

### Overview

#### Framing
Let's start by framing what Solana programming is. Doing this will help us form the mental model of what exactly we're programming.

At the highest-level, you have two kinds of Solana programs:
- an on-chain **program**. Analogous to "smart contract". Literally runs on the blockchain and executes "Solana computer instructions".
- an off-chain **app**. Doesn't run on the blockchain but interacts with the programs that do.

I find this analogous to any client-server interaction model, where the **server (backend)** is the on-chain program and the **client (frontend)** is an app that interacts with it.
In fact, **writing Solana programs is a lot like implementing RPC servers, REST endpoints, [insert distributed / IPC thing here]**.
On-chain programs can interact with other on-chain programs, just like how any server can also be a client.

#### Tooling
Solana supports any programming language that can compile to [BPF bytecode](https://docs.solana.com/developing/on-chain-programs/overview).
However, the vast majority of the ecosystem and tooling is in Rust. **All code in this tutorial is written in Rust, unless otherwise specified.**

Relevant Rust crates:
- `solana-program` for writing on-chain programs
    - in all likelihood, you will also depend on existing programs in the [Solana program library](https://github.com/solana-labs/solana-program-library)
      which are published as their own crates, e.g. `spl-token`
- `solana-sdk`, `solana-client` for off-chain programs

#### On-chain programs
The `solana-program` crate exposes a `macro` aptly named `entrypoint!`

- It follows the framework over library model, i.e. [they call you](https://en.wiktionary.org/wiki/Hollywood_principle)
- So it expects you to pass in a function with a very specific signature. This function is where you will define your logic that ultimately runs on the chain once deployed (more on that later).
- The inputs to this deployed function are passed in via a [transaction](https://docs.solana.com/developing/programming-model/transactions)

- Solana program flow:
    - Process the input, including deserialization to determine the user's instructions
    - Do your logic / algorithm
    - Return the serialized outputs in a `ProgramResult`, a thin-wrapper around `std::result::Result`.
- See the [helloworld example](https://github.com/solana-labs/example-helloworld/blob/master/src/program-rust/src/lib.rs)


### Anchor and Escrow

- Github repo available [here](https://git.w2k.jumptrading.com/chli/anchor-escrow)
  - // TODO transfer to external JumpCrypto Github
- Follow the *quickstart* in the README to quickly get up and running

#### Project structure
In general, it's a good practice to take a layered approach to testing.
Rust's convention is **unit tests** (`#[cfg(test)]`) and **integration tests** (separate `tests` subdirectory).
In addition to this, Anchor supports **end-to-end integration tests** via `anchor test`.

- The Anchor-based "Solana program" lives inside `programs`.
- Run `anchor test` for **Typescript end-to-end integration tests** inside the top-level `tests` directory.
  - These integration tests illustrate how a real "Solana app" might interact with the program.
  - The tests are simple: they just make sure the balances line up after each escrow operation.
- Run `cargo test-bpf` for **Rust-based integration tests** inside `program/tests`.
  - These integration tests illustrate how to use [solana-program-test](https://github.com/solana-labs/solana-program-library) in tandem with [anchor-client](https://github.com/project-serum/anchor/tree/master/client).
  - They follow the Rust convention of "integration tests" inside a sibling `tests` directory to the `src`>
  - It's faster to run pure Rust integration tests, but the Typescript app tests are truly "end-to-end" (includes Anchor IDL).

#### Comparison to Paulx Escrow

I kept the variable names, program structure exactly the same as the original Paulx Escrow tutorial.
This way you can easily switch back-and-forth between the vanilla Solana version and Anchor version and see what's changed.

Here are some differences to highlight from the Paulx tutorial:
- Instead of passing a list of `AccountInfo` and processing them with `next_account_info`, all the passing is handled by `Anchor` via the `struct` definitions `InitEscrow` and `Exchange`
  - Notice how they `#[derive(Accounts)]`: Anchor does the wiring of account passing and parsing for us. We instead just pass in `Context<InitEscrow>`).
- A lot of the checks / constraints are now pulled out of `init_escrow` and `exchange` _functions_ and pushed into the Anchor macros defined on the respective structs.
  - This allows us to separate business logic from administrative logic
- The Escrow account is now created by the Solana program itself (this example) vs. the Javascript client code (Paulx).

> #### Reification
> * The process of turning all the account parsing / processing logic into structs is known as **reification**. We've transformed _code into data_, thereby _reifying_ it.
> * The inverse is **Church encoding**. Turning _data into code_.
> * Read more [here](https://underscore.io/blog/posts/2017/06/02/uniting-church-and-state.html) if this excites you.

_Note that Anchor has their own Escrow program.
Their version implements an escrow program that is structured differently from Paulx: it supports cancellation, and makes use of more advanced and idiomatic Rust.
The below version of escrow using Anchor that maps directly to the Paulx tutorial._

#### Program (Rust)

```rust
use anchor_lang::prelude::*;
use anchor_lang::solana_program::msg;
use anchor_lang::solana_program::program::invoke;
use anchor_lang::solana_program::program_option::COption;
use anchor_spl::token;
use anchor_spl::token::{Token, TokenAccount};
use spl_token::instruction::AuthorityType;
use spl_token::instruction::TokenInstruction::SetAuthority;
use thiserror::Error;

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");

#[program]
pub mod anchor_escrow {
    use anchor_spl::token::accessor::amount;
    use super::*;

    pub fn init_escrow(ctx: Context<InitEscrow>, amount: u64) -> ProgramResult {
        let initializer = &ctx.accounts.initializer;

        let temp_token_account = &ctx.accounts.temp_token_account;

        let token_to_receive_account = &ctx.accounts.token_to_receive_account;

        let escrow_account = &mut ctx.accounts.escrow_account;

        let rent = &ctx.accounts.rent;
        if !rent.is_exempt(
            escrow_account.to_account_info().lamports(),
            escrow_account.to_account_info().data_len(),
        ) {
            return Err(EscrowError::NotRentExempt.into());
        }

        escrow_account.initializer_pubkey = initializer.key();
        escrow_account.temp_token_account_pubkey = temp_token_account.key();
        escrow_account.initializer_token_to_receive_account_pubkey = token_to_receive_account.key();
        escrow_account.expected_amount = amount;

        msg!("Calling the token program to transfer token account ownership...");
        let (pda, bump_seed) = Pubkey::find_program_address(&[b"escrow"], ctx.program_id);
        msg!("PDA {:?}", pda);
        token::set_authority(
            CpiContext::new(
                ctx.accounts.token_program.to_account_info(),
                token::SetAuthority {
                    current_authority: initializer.to_account_info(),
                    account_or_mint: temp_token_account.to_account_info(),
                },
            ),
            AuthorityType::AccountOwner,
            Some(pda),
        )
    }

    pub fn exchange(ctx: Context<Exchange>, amount_expected_by_taker: u64) -> ProgramResult {
        msg!("Calling the token program to transfer tokens to the escrow's initializer...");
        msg!("{:?}", ctx.accounts.takers_sending_token_account.to_account_info().key);
        msg!("{:?}", ctx.accounts.initializers_token_to_receive_account.to_account_info().key);
        msg!("{:?}", ctx.accounts.taker.to_account_info().key);

        msg!("{:?}", ctx.accounts.escrow_account.expected_amount);
        msg!("{:?}", ctx.accounts.pdas_temp_token_account.amount);

        token::transfer(
            CpiContext::new(
                ctx.accounts.token_program.to_account_info(),
                token::Transfer {
                    from: ctx.accounts.takers_sending_token_account.to_account_info(),
                    to: ctx
                        .accounts
                        .initializers_token_to_receive_account
                        .to_account_info(),
                    authority: ctx.accounts.taker.to_account_info(),
                },
            ),
            ctx.accounts.escrow_account.expected_amount,
        )?;
        let (_, bump_seed) = Pubkey::find_program_address(&[b"escrow"], ctx.program_id);
        token::transfer(
            CpiContext::new_with_signer(
                ctx.accounts.token_program.to_account_info(),
                token::Transfer {
                    from: ctx.accounts.pdas_temp_token_account.to_account_info(),
                    to: ctx
                        .accounts
                        .takers_token_to_receive_account
                        .to_account_info(),
                    authority: ctx.accounts.pda_account.clone(),
                },
                &[&[&b"escrow"[..], &[bump_seed]]],
            ),
            ctx.accounts.pdas_temp_token_account.amount,
        )
    }
}

#[derive(Accounts)]
pub struct InitEscrow<'info> {
    #[account(mut)]
    pub initializer: Signer<'info>,
    #[account(mut)]
    pub temp_token_account: Account<'info, TokenAccount>,
    #[account(mut)]
    pub token_to_receive_account: Account<'info, TokenAccount>,
    #[account(init, payer = initializer)]
    pub escrow_account: Account<'info, Escrow>,
    pub rent: Sysvar<'info, Rent>,
    pub token_program: Program<'info, Token>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct Exchange<'info> {
    pub taker: Signer<'info>,
    #[account(mut)]
    pub takers_sending_token_account: Account<'info, TokenAccount>,
    #[account(mut)]
    pub takers_token_to_receive_account: Account<'info, TokenAccount>,
    #[account(mut)]
    pub pdas_temp_token_account: Account<'info, TokenAccount>,
    pub initializer: AccountInfo<'info>,
    #[account(mut)]
    pub initializers_token_to_receive_account: Account<'info, TokenAccount>,
    #[account(
    mut,
    constraint = escrow_account.temp_token_account_pubkey == pdas_temp_token_account.key(),
    constraint = escrow_account.initializer_pubkey == initializer.key(),
    constraint = escrow_account.initializer_token_to_receive_account_pubkey == initializers_token_to_receive_account.key(),
    // constraint = escrow_account.expected_amount == pdas_temp_token_account.amount,
    )]
    pub escrow_account: Account<'info, Escrow>,
    pub pda_account: AccountInfo<'info>,
    pub token_program: Program<'info, Token>,
}

#[account]
#[derive(Default)]
pub struct Escrow {
    pub initializer_pubkey: Pubkey,
    pub temp_token_account_pubkey: Pubkey,
    pub initializer_token_to_receive_account_pubkey: Pubkey,
    pub expected_amount: u64,
}

#[derive(Error, Debug, Copy, Clone)]
pub enum EscrowError {
    /// Invalid instruction
    #[error("Invalid Instruction")]
    InvalidInstruction,
    #[error("Not Rent Exempt")]
    NotRentExempt,
}

impl From<EscrowError> for ProgramError {
    fn from(e: EscrowError) -> Self {
        ProgramError::Custom(e as u32)
    }
}
```

#### End-to-end integration tests (Typescript)

Conveniently, our app will also serve as our "integration test" for the Escrow program above.
It literally does RPC against your program deployed against your Solana localnet.
Recall Anchor generates the scaffolding with `anchor init` and does everything with a single `anchor test`.

```typescript
import * as anchor from '@project-serum/anchor';
import {Program} from '@project-serum/anchor';
import {AnchorEscrow} from '../target/types/anchor_escrow';
import {AccountLayout, Token, TOKEN_PROGRAM_ID} from "@solana/spl-token";
import {
  Connection,
  Keypair,
  LAMPORTS_PER_SOL,
  PublicKey,
  SystemProgram,
  SYSVAR_RENT_PUBKEY,
  Transaction
} from "@solana/web3.js";
import * as assert from "assert";

describe('anchorEscrow', () => {
  // Configure the client to use the local cluster.
  anchor.setProvider(anchor.Provider.env());

  const program = anchor.workspace.AnchorEscrow as Program<AnchorEscrow>;
  const connection = new Connection("http://localhost:8899", 'singleGossip');

  let admin;
  let tokenX: Token;
  let tokenY: Token;
  let initializer: Keypair;
  let initializerTokensX: PublicKey;
  let initializerTokensY: PublicKey;
  let taker: Keypair;
  let takerTokensX: PublicKey;
  let takerTokensY: PublicKey;
  let tempTokenAccount: Keypair;
  let escrowAccount: Keypair;

  const amountX = 42;
  const amountY = 24;

  it('setup', async () => {
    admin = anchor.web3.Keypair.generate();
    await connection.confirmTransaction(await connection.requestAirdrop(
        admin.publicKey,
        LAMPORTS_PER_SOL
    ));

    // Set up Token Mint
    tokenX = await Token.createMint(
        connection,
        admin,
        admin.publicKey,
        null,
        0,
        TOKEN_PROGRAM_ID
    );
    tokenY = await Token.createMint(
        connection,
        admin,
        admin.publicKey,
        null,
        0,
        TOKEN_PROGRAM_ID
    );

    // Alice
    initializer = anchor.web3.Keypair.generate();
    await connection.confirmTransaction(await connection.requestAirdrop(
        initializer.publicKey,
        LAMPORTS_PER_SOL
    ));
    initializerTokensX = await tokenX.createAccount(initializer.publicKey);
    await tokenX.mintTo(initializerTokensX, admin, [], 100);
    initializerTokensY = await tokenY.createAccount(initializer.publicKey);

    // Bob
    taker = anchor.web3.Keypair.generate();
    await connection.confirmTransaction(await connection.requestAirdrop(
        taker.publicKey,
        LAMPORTS_PER_SOL
    ));
    takerTokensX = await tokenX.createAccount(taker.publicKey);
    takerTokensY = await tokenY.createAccount(taker.publicKey);
    await tokenY.mintTo(takerTokensY, admin, [], 100);

    // setting up input accounts for escrow
    // includes setting up Alice's "tempTokenAccount" as described in the tutorial
    // it's kind of pointless in a unit test, but i'm doing it just for completeness
    tempTokenAccount = anchor.web3.Keypair.generate();
    escrowAccount = anchor.web3.Keypair.generate();
    const XTokenMintAccountPubkey = new PublicKey((await connection.getParsedAccountInfo(initializerTokensX, 'singleGossip')).value!.data.parsed.info.mint);
    const createTempTokenAccountIx = SystemProgram.createAccount({
      programId: TOKEN_PROGRAM_ID,
      space: AccountLayout.span,
      lamports: await connection.getMinimumBalanceForRentExemption(AccountLayout.span, 'singleGossip'),
      fromPubkey: initializer.publicKey,
      newAccountPubkey: tempTokenAccount.publicKey
    });
    const initTempAccountIx = Token.createInitAccountInstruction(TOKEN_PROGRAM_ID, XTokenMintAccountPubkey, tempTokenAccount.publicKey, initializer.publicKey);
    const transferXTokensToTempAccIx = Token.createTransferInstruction(
        TOKEN_PROGRAM_ID, initializerTokensX, tempTokenAccount.publicKey, initializer.publicKey, [], amountX);
    const tx = new Transaction().add(createTempTokenAccountIx, initTempAccountIx, transferXTokensToTempAccIx);
    await connection.confirmTransaction(
        await connection.sendTransaction(tx, [initializer, tempTokenAccount], {
          skipPreflight: false,
          preflightCommitment: 'singleGossip'
        })
    );
  })

  it('initEscrow', async () => {
    await program.rpc.initEscrow(
        new anchor.BN(amountY),
        {
          accounts: {
            initializer: initializer.publicKey,
            tempTokenAccount: tempTokenAccount.publicKey,
            tokenToReceiveAccount: initializerTokensY,
            escrowAccount: escrowAccount.publicKey,
            rent: SYSVAR_RENT_PUBKEY,
            tokenProgram: TOKEN_PROGRAM_ID,
            systemProgram: SystemProgram.programId
          },
          signers: [initializer, escrowAccount]
        })
    assert.equal(await getBalance(tokenX, initializerTokensX), 100 - amountX);
    assert.equal(await getBalance(tokenX, tempTokenAccount.publicKey), amountX);
    assert.equal((await program.account.escrow.fetch(escrowAccount.publicKey)).expectedAmount.toNumber(), amountY);
  })

  it('exchange', async () => {
    const [pda, _] = await PublicKey.findProgramAddress(
        [Buffer.from(anchor.utils.bytes.utf8.encode("escrow"))],
        program.programId
    );
    const tx = await program.rpc.exchange(
        new anchor.BN(amountY),
        {
          accounts: {
            taker: taker.publicKey,
            takersSendingTokenAccount: takerTokensY,
            takersTokenToReceiveAccount: takerTokensX,
            pdasTempTokenAccount: tempTokenAccount.publicKey,
            initializer: initializer.publicKey,
            initializersTokenToReceiveAccount: initializerTokensY,
            escrowAccount: escrowAccount.publicKey,
            pdaAccount: pda,
            tokenProgram: TOKEN_PROGRAM_ID
          },
          signers: [taker]
        }
    );
    await connection.confirmTransaction(tx);
    assert.equal(await getBalance(tokenX, initializerTokensX), 100 - amountX);
    assert.equal(await getBalance(tokenY, initializerTokensY), amountY);
    assert.equal(await getBalance(tokenX, takerTokensX), amountX);
    assert.equal(await getBalance(tokenY, takerTokensY), 100 - amountY);
  });
});

const getBalance = async (token: Token, publicKey: PublicKey) => (await token.getAccountInfo(publicKey)).amount.toNumber()
```

#### Local integration tests (Rust, WIP)

```rust
#![cfg(feature = "test-bpf")]

use std::sync::Arc;

use anchor_client::solana_sdk::{system_program, sysvar};
use anchor_lang::prelude::*;
use anchor_lang::{solana_program, AccountDeserialize, AccountSerialize};
use anchor_spl::token::Token;

use anchor_escrow::Escrow;
use {
    anchor_client::{
        solana_sdk::{
            account::Account, commitment_config::CommitmentConfig, pubkey::Pubkey,
            signature::Keypair, signature::Signer, transaction::Transaction,
        },
        Client, Cluster,
    },
    anchor_escrow,
    solana_program_test::{tokio, ProgramTest},
    std::rc::Rc,
};

#[tokio::test]
async fn init() {
    let mint_account = Keypair::new();
    let mint_authority = Keypair::new();
    let mint_authority_pubkey = mint_authority.pubkey();

    let initializer = Keypair::new();
    let temp_token_account = Pubkey::new_unique();
    let token_to_receive_account = Pubkey::new_unique();
    let escrow_account = Keypair::new();

    let mut pt = ProgramTest::new("anchor_escrow", anchor_escrow::id(), None);
    pt.add_account(initializer.pubkey(), Account::default());
    let (mut banks_client, payer, recent_blockhash) = pt.start().await;

    let client = Client::new_with_options(
        Cluster::Debug,
        Keypair::new(),
        CommitmentConfig::processed(),
    );

    // TODO setup incl. minting token, etc.

    let program = client.program(anchor_escrow::id());
    let ix = program
        .request()
        .accounts(
            anchor_escrow::accounts::InitEscrow {
                initializer: initializer.pubkey(),
                temp_token_account,
                token_to_receive_account,
                escrow_account: escrow_account.pubkey(),
                rent: sysvar::rent::id(),
                token_program: anchor_spl::token::ID,
                system_program: system_program::ID,
            }
            .to_account_metas(None),
        )
        .args(anchor_escrow::instruction::InitEscrow { amount: 1 })
        .instructions()
        .unwrap()
        .pop()
        .unwrap();

    let transaction = Transaction::new_signed_with_payer(
        &[ix],
        Some(&payer.pubkey()),
        &[&payer, &initializer, &escrow_account],
        recent_blockhash,
    );

    banks_client.process_transaction(transaction).await.unwrap();
}
```




### Walkthrough: Anchor and Escrow (WIP)
_Anchor is a framework for building and interacting with smart contracts on Solana. Think Ruby on Rails for Solana._

"Vanilla" Solana programming is pretty low-level: you have to remember the position order of `accounts`, manually serialize/deserialize, etc.
As programmers, we understand too well the value of **abstraction**.
When was the last time you thought about what x86 instructions your Python programming is executing? 
That's hyperbolic, but you get the point: Anchor is a framework roughly at the same level of what Flask does for Python web dev.

But Anchor is more than an abstraction layer: it supercharges your workflow, enabling rapid-fire development.
A single CLI command `anchor test` will build, deploy and "integration test" your Solana program all at once!
Remember when you first discovered **hot-reloading** your web app with every code change? This feels exactly the same.

#### Goals
1. highlight why Anchor makes Solana programming easier by rewriting Escrow
2. peel back Anchor's abstraction layers and understand how it translates to vanilla Solana
3. make you "productive" ASAP, because now you write Solana programs with an automated feedback loop

TODO notes:
- abstract away input validation, ceremony, administrative vs. business logic, analogize to decorators in python

#### Prereqs
Run through `Getting Started` and `Tutorials` in the sidebar [here](https://project-serum.github.io/anchor/getting-started/introduction.html).

I assume also you've worked through the Escrow tutorial:
* https://paulx.dev/blog/2021/01/14/programming-on-solana-an-introduction/
* TODO link to our Escrow post

#### Setup
If you ran through the above prereqs, then `anchor` is available as a command in your env.

You should also be familiar with tokens and how to set them up. One way is through the CLI:

```sh
# these commands all take arguments, but it's pretty intuitive what to do
spl-token create-token
spl-token create-account
spl-token mint
spl-token transfer
```

Another way is UI tools: see the [dev tools](#devtools) above. It's easy to forget to set mainnet-beta to localnet, so if it ever seems like data is missing from Web UIs check that first.

**You won't need to setup tokens for this Anchor Escrow walkthrough -- instead we'll create them in the RPC client with every run of `anchor test`.**

#### Step 1: Rapid-fire development
Inside a directory:
* `anchor init anchor-escrow`
* `anchor test`

> You can also choose to `anchor test --skip-local-validator`.
> This option makes it so Anchor uses your existing Solana localnet vs. anchor spinning one up for the `test` workflow.
> We're **not** going to use our own localnet, instead letting `Anchor` do that for us.

One great feature of Anchor is rapid development cycles. To achieve that, we want to get `anchor test` in a good place.

> What happened when you did these two steps?

* Anchor generated both a Solana program and integration test client for you.
  * The Solana program is a trivial "hello world" program. It's takes in an empty struct as input and does a `no-op`.
  * The integration test client `anchor-escrow.ts` is a Typescript program that does RPC against your Solana program.
* Anchor then ran a test using `mocha` (JS testing framework) to verify the behavior of your program.
  * Since your program is a no-op, the test pretty much always passes as long as it can connect to the Solana localnet
  
This should mostly be familiar, if you worked through the above tutorial.

As you can imagine, this enables a very fast feedback-loop:
`change Solana program code -> change JS test -> anchor test -> repeat`


#### Step 2: Peel back abstraction

```typescript
const tx = await program.rpc.initialize({});
```

Anchor magically setup this RPC call for you in the `anchor-escrow.ts`. What's actually happening?
I find a good way to peel back abstraction is to implement the vanilla version yourself.

```typescript
import {Connection, Transaction, TransactionInstruction} from '@solana/web3.js';


const conn = new Connection("http://localhost:8899", "singleGossip")
const ix = new TransactionInstruction({
  programId: "Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS",
  keys: [],
  data: null
})
const tx = new Transaction(ix);
await conn.sendTransaction(tx)
```

Not so fast! The above code snippet has a bug. Try running `anchor-test` yourself. Can you spot them?

<details>

The error message you get should make the problem apparent: there's no signers.

```
  anchor-escrow
    1) Is initialized!


  0 passing (89ms)
  1 failing

  1) anchor-escrow
       Is initialized!:
     Error: No signers
      at Transaction.sign (node_modules/@solana/web3.js/src/transaction.ts:450:13)
      at Connection.sendTransaction (node_modules/@solana/web3.js/src/connection.ts:3615:21)
      at processTicksAndRejections (node:internal/process/task_queues:96:5)
```

The solution is to add a signer. Changing the line to:

```typescript
await conn.sendTransaction(tx, [aliceAccount]);
```

should do the trick.


</details>

There's another problem. Run `anchor test --skip-local-validator` again. Any ideas?

<details>

```
Error: Account 37P5L6oGSYxcMVyqqeXkrPABGF1aShA2aL5RydzECFkM has insufficient funds for spend (1.06357848 SOL) + fee (0.000775 SOL)
There was a problem deploying: Output { status: ExitStatus(unix_wait_status(256)), stdout: "", stderr: "" }.
```

Recall from the Anchor website's tutorial that Anchor accounts are prepended with an `8-byte-discriminator`.
Anchor relies on this to add "types" over Solana accounts.

This applies for instructions too: the `data` field is `null` above because our no-op program `initialize` also takes 0 arguments,
but you still need to add the discriminatory like so:

```typescript
data: Buffer.from(sha256.digest("global:initialize")).slice(0, 8)
```

</details>

Ok, we're good now. Recall from the Paulx Escrow tutorial that there was a bunch the `app` needed to do vs. the `program`.
If you look at the Escrow tutorial [client-side code](https://github.com/paul-schaaf/solana-escrow/blob/master/scripts/src/alice.ts):

`alice.ts` corresponds to the `initEscrow` instruction.

So let's set up that same instruction using `Anchor`. First defining a struct:

```rust


```

TODO continue walkthrough


### Walkthrough: Serum (WIP)

Let's take a look at something more complicated than Escrow. How about the [Serum dex](https://github.com/project-serum/serum-dex) itself!

To begin, let's examine the project structure. It looks like a monorepo: both the on-chain *program* and the *app* live in the same codebase.

Ok, now let's look for the entrypoint to the on-chain program. Remember that `entrypoint!` macro mentioned earlier? It's being called in `dex/src/lib.rs`:

```rust
#[cfg(all(feature = "program", not(feature = "no-entrypoint")))]
use solana_program::entrypoint;
#[cfg(feature = "program")]
use solana_program::{account_info::AccountInfo, entrypoint::ProgramResult, pubkey::Pubkey};

#[cfg(feature = "program")]
#[cfg(not(feature = "no-entrypoint"))]
entrypoint!(process_instruction);
#[cfg(feature = "program")]
fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    instruction_data: &[u8],
) -> ProgramResult {
    Ok(state::State::process(
        program_id,
        accounts,
        instruction_data,
    )?)
}
```

Simple enough. Continuing to follow the white rabbit into the `process` method of `state.rs`:

```rust
#[cfg_attr(not(feature = "program"), allow(unused))]
impl State {
    #[cfg(feature = "program")]
    pub fn process(program_id: &Pubkey, accounts: &[AccountInfo], input: &[u8]) -> DexResult {
        let instruction = MarketInstruction::unpack(input).ok_or(ProgramError::InvalidArgument)?;
        match instruction {
            MarketInstruction::InitializeMarket(ref inner) => Self::process_initialize_market(
                account_parser::InitializeMarketArgs::new(program_id, inner, accounts)?,
            )?,
            MarketInstruction::NewOrder(_inner) => {
                unimplemented!()
            }
            MarketInstruction::NewOrderV2(_inner) => {
                unimplemented!()
            }
            MarketInstruction::NewOrderV3(ref inner) => {
                account_parser::NewOrderV3Args::with_parsed_args(
                    program_id,
                    inner,
                    accounts,
                    Self::process_new_order_v3,
                )?
            }
        ...
```
Interesting! So it looks like they deserialize the instructions then pattern match to figure out what the client wants to do.

As you might expect, these are standard instructions you'd run on an order book.

### APIs and the bigger picture

One thing that stands out here is the `NewOrder`, `NewOrderV2`, `NewOrderV3` definitions, with the first two `unimplemented`.
It looks a little funky. It appears to be handling for API versioning / compatibility.

Here is a key point: *a Solana program is ultimately an API*. As will all APIs, it's important to think carefully about the interface you're providing.
- What are you going to do when your program logic changes?
- How are you going to add features (API additions)?
- How are you going to deprecate functionality (breaking changes)?

#### Ok, so why is this important?

When there is an entire ecosystem of other programs or client apps that depend on your on-chain program, it's _especially_ critical to consider these points.

Imagine a dApp developer building a UI on top of the Serum interface.
How would they feel if Serum was constantly changing the way they implemented order submission,
breaking their app? They'd probably stop using Serum.

In the spirit of decentralization, you should consider it _your_ responsibility to think carefully about your consumers everytime you make a change your program.
Did the public API change? Is it really encapsulated from the client?
A culture of constant breaking changes will damage morale and eventually drive developers away from the ecosystem.

All this said, as with anything there is a trade-off. Solana programming is still very new.
Development is rapid and ongoing. That comes hand-in-hand with breaking changes.

In my opinion, the best thing to do is think for yourself: carefully consider the trade-offs and make an informed decision.
Don't blindly break APIs, thinking that's ok just because Solana development is new.
Don't chain yourself to previous iterations of your code either -- if your app has evolved, your codebase should evolve with it.
Solana explicitly spells out some [backwards compatibility guidelines](https://docs.solana.com/developing/backwards-compatibility).


### API Evolution in Serum (WIP)

Take a look at this pull request releasing Dex V3, a **breaking change**:
https://github.com/project-serum/serum-dex/pull/97. Here's the description:

> The primary change is to immediately match incoming orders against the book,
> instead of first buffering them in the request queue.
> The request queue still exists to reduce breakage, but is always empty.
> Because of this, we're forced to remove support for the old order placement and cancellation instructions,
> since they don't provide the bids and asks accounts which would be necessary in order to process them in the new model.

Some more context on the purpose of the above change:
- orders used to go into a `request queue`, not the order book directly.
- a "user" (i.e. either another on-chain program or client app) would explicitly send a "crank turn" instruction to Serum
- this "crank turn" pulled requests off the queue and matched them in the order book

PR Observations:
- All affected instructions implemented a corresponding new version "V2" or "V3", e.g. `MarketInstruction::CancelOrderV2`
- The old versions of the instruction were then removed by replacing the body with `unimplemented!`

### Solana programming pros and cons

_LISP programmers know the value of everything and the cost of nothing_

Solana programs are **stateless**. What does that mean?
Programs cannot hold state, so it's passed in via `process_instruction`.

The consequence is the same input always results in the same output.
If you're familiar with the "pure functional programming" paradigm, you'll feel right at home.

**Not every chain enforces statelessness. What are the pros / cons to Solana doing this?**

Pros:
- Parallel processing. Sealevel (Solana runtime) knows exactly what data a program depends on and can safely parallelize
- Stateless-ness and determinism makes it easier to reason about programs, because of [referential transparency](https://en.wikipedia.org/wiki/Referential_transparency)

Cons:
- Debugging can be difficult because a lot of data lives outside your program that you have to fetch with RPC
- APIs for passing around accounts are not that friendly: they're passed as an array, so you have to remember the position-order

[Anchor](https://project-serum.github.io/anchor/getting-started/introduction.html) aims to solve some of the cons described.
See the above Anchor escrow tutorial as well as [angkor wat](https://2501babe.github.io/posts/anchor101.html).


### Accounts (WIP)

- Accounts are just bytearrays `&[u8]`.
- Accounts have a `data` field for you to store arbitrary information
- Executable accounts are programs
- Types of account "types" in Anchor: Account, Program, Sysvar, different macros
- Go into how Serum parses the account array into the literal information it needs to execute the order instruction



### Highlights

> Solana programs are stateless

> If the program needs to store state between transactions, it does so using accounts

> Programs are constrained to run deterministically, so random numbers are not available

> the basic operational unit on solana is an instruction. an instruction is one call into a program.
> one or more instructions can be bundled into a message. a message plus an array of signatures constitutes a transaction


It's worth reviewing the [on-chain programming docs](https://docs.solana.com/developing/on-chain-programs/overview)
or at least the [FAQ](https://docs.solana.com/developing/on-chain-programs/faq).
It'll save you a debugging headache.

### Gotchas
- Don't use std::collections::HashMap. You'll get an obscure error because of the "no-randomness" constraint
    - Reason: `HashMap<K, V, S = RandomState>`. Notice the generic type `S` defaults to `RandomState`
    - It may be possible use by substituting a different, non-random `S` - see the `with_hasher` constructor (*I have not tried this myself*)
- Relatedly, don't use `rand` crate. If a crate you depend on transitively depends on `rand`, follow [this guide](https://docs.solana.com/developing/on-chain-programs/developing-rust#depending-on-rand)
