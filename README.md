# LongHash CryptoCon Vol.2 Hackathon - Challenge #3

## Table of Contents
  * [Chapter 1 - Problem](#chapter-1)
  * [Chapter 2 - Solution](#chapter-2)
  * [Chapter 3 - References](#chapter-3)

### Problem <a id="chapter-1"></a>

The problem is _"direct anonymous attestation of membership proof"_ (Steven Pu, Founder & CEO, Taraxa), 17th May 2019 [1], where:

* Manufacturer gives each of its specific machines a unique identity.
* Manufacturer may not have control its machines.
* Manufacturer is unable to trust the identity of machines since:
  * User may have generated a random number matching an identity.
  * User may have transplanted the identity into a different kind of machine
  * User may have try to clone an identity.

### Solution <a id="chapter-1"></a>

#### Introduction
Solve the problem through _"practical use of a zero-knowledge protocol so machines may be trusted for machine-to-machine transactions"_ (Steven Pu, Founder & CEO, Taraxa), 17th May 2019.

#### Background Research

##### Confidential Transactions (CT)

Configential Transactions (CTs) may hide transaction input and output amounts from public view by including in each transaction submitted for public blockchain validation a zero-knowledge "proof of validity" of the CT [2, pg. 1 & 3], as a hidden Pederson commitment to the amount.

##### Trusted Setup

CTs where the ZKP requires "trusted setup" (i.e. SNARKS) are not desirable since all users must trust that the setup was performed correctly [2, pg. 1].

CTs where the ZKP does not require "trusted setup" (i.e. STARKS) may not be desirable, since although they are efficient, their Zero-Knowledge Range Proof (ZKRP) (over committed values), which are the main contributor to the size (kB) of a CT, may be larger than with SNARKS [2, pg. 1 & 3].

##### NIZK Proofs

Smart contracts are public blockchain transactions and provide no privacy [2, pg. 5]. Non-Interactive Zero-Knowledge (NIZK) [2, pg. 5] may be used to prevent leaking smart contract inputs (user machine information), however verification of the proof is expensive and may not yet be suitable for conputation by smart contracts on Ethereum.

##### Zero-Knowledge SNARKs Proofs

SNARKS (Succinct Non-Interactive Arguments of Knowledge) have succinct (small message size in comparison with computation length) messages [5, pg. 3], and fast verification times, but they are non-interactive since there is usually an initial "trusted setup" phase followed by a single message from the "prover" to the "verifier" [5, pg. 3], and long Common Reference Strings (CRS), which are require computationally intensive protocols to generate distributively, and are required for each smart contract [2, pg. 5 & 6] and should be known by both the "verifier" and "prover".

##### Bulletproofs

Bulletproofs uses small proofs without any "trusted setup", but may not be cheap [2, pg. 6].

Smart contracts may be used only to verify the validity of the Decentralised Certificate Authority's proof if challenged by another party [2, pg. 6].

Tokens may be used by the manufacturer to incentivise behaviour by slashing parties that create incorrect proofs or challenge correct proofs (similar to governance in Polkadot). The Interactive Referee Delegation Modal may be used to further improve the privacy preserving nature of the smart contract. See [2, pg. 6] for further information about this model.

#### Proposed Zero-Knowledge Protocol Solution

##### Provisions Protocol using Bulletproof ZKPs (without Trusted Setup)

Provisions Protocol [2, pg. 4] has allowed exchanges to prove their solvency, without revealing additional information by using ZKRPs to prevent the insertion of fake accounts with negative balances, but where the size of the ZKRP is linear to the number of the exchange's customers.

Bulletproof ZKPs without "trusted setup" [2, pg. 1] offers the following features that can achieve a 300x reduction in the size of the Provisions Protocol ZKRP:

* Short ZKP, where a secret value was commited in a given interval
* Non-Interactive Zero-Knowledge (NIZK) Proofs by using the Fiat-Shamir heuristic
* Support a multi-party computation (MPC) protocol, which allows the "prover" of a CT with multiple outputs from multiple parties each having committed secret values to jointly generate a single small aggregated ZKRP for all their values, without revealing their secret values [2, pg. 3].
* Support "batched verification" of multiple Bulletproofs to reduce overall cost of verifying each additional proof, **FIXME - is the rest of this dot point valid?** with potential integration with a Decentralised Certificate Authority.
* Particularly applicable to sharded chains (side-chains, parachains) and private blockchains.

Manufacturers have a set of machines that are part of their product suite. They should use Zero-Knowledge Set Membership Proof (ZKSMP) [3, README.md] as a replacement for its subset ZKRP to identify and commit to a "secret" and the range of values that the "secret" fits within in consultation with their trusted parties, and then generate a ZKSMP from that commitment and embed it into the machine so the blockchain network may use it to validate the machine's identity through verifying the ZKSMP that confirms the values that the "secret" fits within.

##### Tokenised Business Model

* Manufacturers have a list of all the key pairs associated with each machine they have manufactured.
* Machines should generate ephemeral cryptographic keys locally (generated for each execution of a key establishment process), and change it periodically or when their certificate that's on the blockchain expires.
* Sellers are machine owners that each have an IP address and sell their data (i.e. sending data about their vehicle, smart buildings, decentralised car sharing, etc).
* Manufacturers and other entities (i.e. buyers and sellers) only communicate through TOR, or similar equivalent, to obscure their IP address.
* Machines (i.e. Google Home, Amazon Echo, or similar), upon their initial deployment, automatically sends a request to the Manufacturer's fixed public IP address to obtain a certificate from its Decentralised Certificate Authority to establish its valid identity, and make its data available for sale or trading).
Traditionally a manual person-to-person process of negotiation was performed by a systems integrator, who negotiates with each machine OEM to obtain access to its data [1, @reedvoid].
* Manufacturers construct a ZKSMP challenge that is answerable by each machine using a mechanism in order to verify the authenticity of the machine, confirming that the machine was manufactured by them, that it's identity was not generated by a Random Number Generator (RNG), and whilst maintaining privacy of the registered seller data (anonymity of the machine's location, ownership, etc) so that other entities may trust and trade with it.
* Manufacturers (large semiconductor companies that build processors or radios) need to be given an incentive to implement this in the long-term, so in the short-term this may not occur so it may be necessary for manufacturers to build their own hardware and distribute it.
* Pool the data collected from the anonymous sellers that have registered.
* Incentivize registered sellers with rewards from the pool for sharing their data. 
* Buyers may sell the pool of data collected to various use cases and perform Federated Learning (distributed machine learning).

#### Decentralised Machine Setup

##### Embed Encrypted Keys into Machines

* Manufacturer embeds a pair of asymmetric encryption keys on the discrete set of machines that are part of their product suite.

##### Decentralised Certificate Authority

* Manufacturer constructs a temporary X.509 certificate on the machine, which contains the public key associated with the machine's identity, so that a proof does not need to be provided every time, temporary because the end user might want to stop even anonymous data collections.

###### Important Notes:

* Decentralised certificate authorities have a decent storage and retrieval mechanism that provides an alternative to using traditional centralised certificate authorities by offering the following benefits:
  * Avoid expensive upfront costs of top-tier centralised certificate authorities.
  * Avoids risks associated with having to remember keys or passwords, hiring security experts to guard them.
  * Avoid risks associated with botnets.
* X.509 digital certificates use the international X.509 public key infrastructure (PKI) standard to verify that a public key belongs to the identity contained in the certificate.
* Manufacturers discussed in this context are willing to disclose the public keys of all its products openly.

##### Remote Anonymous Identity Verification & Machine Data Collection Scheme

Manufacturer or other entities performing transactions with machines are able to anonymously collects data from them without knowing exactly which machine it came from.

* Manufacturer or other entity "verifier" may engage in secure communication with a machine "prover" to perform authentication via a zero-knowledge proof (ZKP) that verifies their authenticity (without requring any passwords to be exchanged).
* Proof that the machine knows one or more "secrets" must be provided in response to a challenge that demonstrates it is indeed one of the machines the manufacturer created in their product suite, but with zero-knowledge revealed about the device's public key identity or the "secret" encryption keys that they embedded, to convince the "verifier" that they know the "secret".
**FIXME - manufacturers discussed in this context are willing to discloe the public keys of all their products openly, so surely i'm incorrect here in stating that they want zero-knowledge revealed about the device's public key identity**
* Verifier that is challenging the authenticity of the machine checks that data received is not corrupt.

###### Important Notes:

* Hacking the machine on-premise to obtain these keys is prohibitively expensive to do (i.e. transport logistics costs of an in-house inspection).
* Data collection must provide cryptographic-guarantee that data collection from user machines without their consent maintains their anonymity (impossible to trace their identity).

##### Deployment of the Verifiers Challenge and the Machines Proof onto a Blockchain

###### Goals

* Manufacturer and machine both anchor the challenge and proof onto the blockchain.
* Machine anchors its data transmissions onto the blockchain with the temporary certificate.

###### Proposed Implementation

Creation of a Substrate parachain whose runtime compiles to WASM and that uses the a custom Bulletproof-specific runtime module that may be later ported over to the Polkadot network. Model the circuit using Circom [7].

This may avoid encountering computational limitations whilst Ethereum Just-in-time (JIT) compilation and interpretation time improvements are being undertaken or until a switch from the EVM to eWASM is made. It also depends on the guaranteed performance of the Ethereum Virtual Machine (EVM) or the Polkadot Runtime Environment (PRE) and the runtime of the manufacturer's algorithm that is being used [5, pg. 4 & 5] when trying to verifying zero-knowledge proofs using Ethereum or Substrate/Polkadot smart contracts (i.e. a custom range proof validator similar to the example Solidity contract of [3, RangeProofValidator.sol] that may be deployed on the Ganache Testnet using the Truffle framework).

#### Additional Security & Energy Efficiency Initiatives

##### Limited-Use Off-Peak Transaction Processing

_"Banks have multiple layers of security, built over 60+ years. They're only online for a few milliseconds each day when transactions are initiated (as payments are only made once per day). Hacks would have to happen from the inside, but the system is fragmented so that'd take years. People still lose money with banks because customers lose or give away their credentials"_, John Calian, Senior Vice President, T-Mobile, 17th May 2019 (modified by Luke Schoen) [1].

* Manufacturers and other entities performing off-chain transactions with machines may consider processing low-priority transactions and storing the verifiers challenge and the machine's proof on-chain using smart contracts on sharded chains (parachains) that are only on-line during a limited off-peak periods to reduce the impact on the electricity grid, to reduce energy costs, and to further increase security (less online exposer).

##### Critical Infrastructure

_"We can't use threshold signing for large transaction amounts since the sender may be running a validator. Validation of large sums must be centralised (by connecting secure hardware modules that participate), and may require increasing the number of signatures"_, Peter Czaban, Executive Director, Web3 Foundation, 17th May 2019 (modified by Luke Schoen) [1].
**FIXME - expand on what was said, is this entirely relevant to this challenge?**

* Manufacturers may need to consider requiring more signatures from validators and the use of VDFs or similar equivalent for transactions that involve large sums, or involve machines that are critical infrastructure.

### References:  <a id="chapter-3"></a>

* [1] Hackathon - https://hack.longhash.com
* [2] Bulletproofs: Short Proofs for Confidential Transactions and More - 2017-1066.pdf
* [3] Code - Range Proof Validator implementations - https://github.com/ing-bank/zkproofs
* [4] Paper - Efficient Protocols for Set Membership and Range Proofs, Camenisch, J. et al., https://infoscience.epfl.ch/record/128718/files/CCS08.pdf
* [5] Paper - zkSNARKs in a Nutshell, Reitwiessner, C., ZK Study Group (of the Zero Knowledge Podcast)
* [6] Website - Xain AG (use their business model as contents)
* [7] Circom - https://github.com/iden3/circom
* [8] Zocrates - https://github.com/Zokrates/ZoKrates

### Key Contacts

* @reedvoid, Steven Pu, Founder & CEO, Taraxa
* Trevor Oakley, Team Member
