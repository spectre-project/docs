# Spectre Documentation

[This repo is a work in progress]

[![Join the Spectre Discord Server](https://img.shields.io/discord/1233113243741061240.svg?label=&logo=discord&logoColor=ffffff&color=5865F2)](https://discord.com/invite/FZPYpwszcF)

## Overview

Spectre on Rust is a fork of [Kaspa on Rust](https://github.com/kaspanet/rusty-kaspa)
introducing CPU-only mining algorithm [SpectreX](https://github.com/spectre-project/rusty-spectrex).

[SpectreX](https://github.com/spectre-project/rusty-spectrex) is based on [AstroBWTv3](https://github.com/deroproject/derohe/tree/main/astrobwt/astrobwtv3)
and proof-of-work calculation is done in the following steps:

* Step 1: SHA-3
* Step 2: AstroBWTv3
* Step 3: HeavyHash

Spectre will add full non-disclosable privacy and anonymous
transactions in future implemented with the GhostFACE protocol
build by a team of anonymous crypto algorithm researchers and
engineers. Simple and plain goal:

* PHANTOM Protocol + GhostDAG + GhostFACE = Spectre

Spectre will become a ghostchain; nothing more, nothing less. Design
decisions have been made already and more details about the GhostFACE
protocol will be released at a later stage. Sneak peak: It will use
[Pedersen Commitments](https://github.com/dalek-cryptography/bulletproofs)
as it allows perfect integration with the Spectre UTXO model and
allows perfect hiding. ElGamal will be used for TX signature signing
as it has a superior TPS (transactions per second) performance. Any PRs
are welcome and can be made with anonymous accounts. No pre-mine, no
shit, pure privacy is a hit!

## Comparison

Why another fork? Kaspa is great but we love privacy, Monero and DERO
are great but we love speed! So lets join the cool things from both.
We decided to take Kaspa as codebase, quick comparison:

Feature                      | Spectre  | Kaspa      | Monero  | DERO
-----------------------------|----------|------------|---------|-----------
PoW Algorithm                | SpectreX | kHeavyHash | RandomX | AstroBWTv3
Balance Encryption           | Future   | No         | Yes     | Yes
Transaction Encryption       | Future   | No         | Yes     | Yes
Message Encyrption           | Future   | No         | No      | Yes
Untraceable Transactions     | Future   | No         | Yes     | Yes
Untraceable Mining           | Yes      | No         | No      | Yes
Built-in multicore CPU-miner | Yes      | No         | Yes     | Yes
High BPS                     | Yes      | Yes        | No      | No
High TPS                     | Yes      | Yes        | No      | No

Untraceable Mining is already achieved with AstroBWTv3 and a multicore
miner is already being shipped with Spectre, working on ARM/x86. We
leave it up to the community to build an highly optimized CPU-miner.

## Mathematics

We love numbers, you will find a lot of mathematical constants in the
source code, in the genesis hash, genesis payload, genesis merkle hash
and more. Mathematical constants like [Pi](https://en.wikipedia.org/wiki/Pi),
[E](https://en.wikipedia.org/wiki/E_(mathematical_constant)) and
several prime numbers used as starting values for nonce or difficulty.
The first released version is `0.3.14`, the famous Pi divided by 10.



## Table of Contents

1. [Getting Started](Getting%20Started/README.md)
   * [Rust Full Node Installation](Getting%20Started/Rust%20Full%20Node%20Installation.md)
   * [Hashrate-data.json](Getting%20Started/hashrate-data.json)
