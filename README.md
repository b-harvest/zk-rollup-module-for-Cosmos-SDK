# zk-rollup-module-for-Cosmos-SDK

This repository is for explaining objectives and technical description of how B-Harvest will implement zk-rollup module for Cosmos-SDK

## Short Summary

zk-rollup methodology utilizing zk-SNARKs provide an efficient way to scale out from the Cosmos Hub (the Hub = Layer 1 = L1) to an application specific Cosmos Zone (a Zone = Layer 2 = L2) without sacrificing the security of the Hub.

In order to give the Hub ability to support any governance-allowed Zone with zk-rollup security enhancement, the Hub should adopt a generalized zk-rollup module which will provide zk-proof verification and merkle tree management functionality.

In this project, B-Harvest will provide "full zk-rollup package" to provide the community full development cycle to adopt additional zk-rollup supported Zone.

## Background Technology

There exists multiple toolkits for zk-SNARKs. To name a few, 

- [https://github.com/scipr-lab/libsnark](https://github.com/scipr-lab/libsnark) : C++
- [https://github.com/zcash/zcash](https://github.com/zcash/zcash) : C++
- [https://github.com/barryWhiteHat/roll_up](https://github.com/barryWhiteHat/roll_up) : C++
- [https://github.com/zkcrypto/bellman](https://github.com/zkcrypto/bellman) : rust
- [https://github.com/Zokrates/ZoKrates](https://github.com/Zokrates/ZoKrates) : rust
- [https://github.com/matter-labs/zksync](https://github.com/matter-labs/zksync) : rust
- [https://github.com/Loopring/protocols](https://github.com/Loopring/protocols) : C++
- [https://github.com/ConsenSys/gnark](https://github.com/ConsenSys/gnark) : golang

B-Harvest investigated many existing implementations and found out that the "gnark" implemented by Consensys is very concise, no library dependency, language compatible to Cosmos-SDK, performance optimized, and also providing relevant tools for simple usecase of zk-SNARKs and zk-rollup.

To not re-invent the wheel, we will actively re-utilize many parts of gnark implementation to create entire zk-rollup package customized to Tendermint/Cosmos-SDK based blockchain usecase.

The gnark repository includes 

- Groth16 implentation with
    - prover and verifier programs
    - ellptic curves : Edward twisted BN256, Edward twisted BLS381
    - merkle tree hash : MiMC
- Circuit programming tool
    - conversion system from constraint system (in golang) into R1CS
    - necessary circuit gadgets for elliptic curves and merkle tree hashing
    - pseudo zk-SNARKs setup script to output prover&verifier key

## Range of Spec

### Mileston 1 : Technology Package Delivery

**Duration**

- maximum 5 months

**Backend**

- zk-SNARKs tools for Cosmos ecosystem use-case
    - zk prover and verifier program
        - Groth16 implementation with default chosen elliptic curve and its pairing computation
- Blockchain module
    - zk rollup module for the Hub
        - zk-proof verification and merkle tree management functionality
        - store current prover&verifier key
- testcodes for each functionality provided

**Frontend and Tools**

- zk tools
    - zk-SNARKs setup script for customized Cosmos usecase
- circuit programming gadgets
    - circuit program for signature verification with default chosen elliptic curve
    - circuit program for hash verification with default chosen merkle tree
    - example circuit program for plain vanilla ledger merkle tree with transfer utility and signature verification ability

**Documentation, Marketing and Governance Effort**

- Technology documentation describing the structure of whole package and list of related crypto technology employed and its mathematical security characteristics
- zk-rollup Explainer documentation and marketing contents for broader non-technical community members
- Various community engagement regarding spreading out benefits of zk-rollup on the Hub
- Related governance leadership to include zk-rollup module on the Hub

### Milestone 2 : Governance Process and Production Support

**Duration**

- Maximum 3 months

**Backend**

- Governance Process
    - Process for add new zk-rollup supported Zone or Zone upgrade process
        - update prover&verifier key and reset merkle tree to genesis state of the Zone via governance procedure
        - the Hub does not need any software upgrade

**Example Codes and Test Suite**

- Example Daemons for plain vanilla Zone and related zk-rollup functionality
    - Customized IBC relayer daemon for communication between the Hub and the Zone
    - Prover daemon
        - listen from the Zone's LCD
        - calculate zk-proof for committed blocks on the Zone blockchain
        - broadcast zk-proof transaction onto the Zone blockchain
    - Node client daemon for the Zone
        - merkle tree management for zk-proof
        - zk-proof verification and storage into kv-store
    - Basic web interface for monitoring and testing entire zk-rollup process

**Documentation and Simulation Report**

- Additional Documentations
    - detail documentation to explain each step to deploy zk-rollup supported Zone on the Hub or other Cosmos blockchains
    - documentation about steps for entire flow testing
- Simulation Report
    - Performance zk-rollup test result with default vanilla Zone
        - prover&verifier performance with thousands of transaction in a block

### Further Roadmap not Included in this Funding Project

- Privacy-preserving Zone with zk-rollup support from the Hub
- Performance optimization of zk prover
    - Parallel computing algorithm
    - Adopting alternative elliptic curves
    - Adopting alternative zk-SNARKs methodology
- Support signature verification functionality for multiple additional elliptic curves

## Team Due Diligence / Track Record

**Prior ICF funding history**

- Four pillars for validator operation
    - Double-singing risk reduction (PR ready)
    - Improving trusted peering (Merged)
    - Tendermint mode (PR ready)
    - Validator consensus key rotation (under development)

**Strong Motivation and Sustainable Management**

- B-Harvest is one of the most devoted community member of Cosmos Network who believes Defi service with Hub-level security is crucial necessary condition to compete other Defi services outside Cosmos ecosystem
- B-Harvest is implementing zk-rollup supported zone which will provide superior performance to users without sacrificing Hub-level security or congestion of the Hub with many transactions. For this roadmap, generalized zk-rollup functionality of the Hub is the most crucial step. So, we are not only motivated but we naturally have strong incentives to provide zk-rollup module with best quality for our project.
- zk-rollup supported zone roadmap also gives us synergetic reasons why we want to keep managing and improving the zk-rollup package. We hope to keep spreading the advantages of zk-rollup technology in Cosmos ecosystem and become a helpful guide to introduce and provide the functionality and tools
- We have strong willingness to accept additional funding from ICF or community fund to extend our contribution to zk-rollup package by implementing further roadmap topics so that our module can keep track with global zk-rollup frontiers

**Skills and Man power**

- Experience on Tendermint and Cosmos-SDK
    - Four pillars for validator operation
    - Kava audit
    - Terra grant for oracle vulnerability
- Crypto SDK implemented for advanced encryption schemes
    - secret sharing, proxy re-encryption, AEAD, KEM
- Man power
    - Mathematics expert for advanced elliptic curve utilization
        - Hyung(BS.math), Dokyung(PhD.math)
    - Tendermint/Cosmos-SDK specialists
        - Dongsam(MS.computer science), Jeseon(MS.computer science)
    - expert crypto engineer
        - Hanjun(BS.computer science)

**Testability and Open source**

- Testing
    - Testing consistency of zk-SNARKs process by testcodes
    - Guidelines and automated testing scripts for entire process
- Open source
    - The repository on github will be open source from begining

## Budget

**Man power**

- Hyung(Full-time) : spec design, mathematics and cryptography verification, documentation, marketing and communication
- Dongsam(Full-time) : zk-rollup module development, governance module update
- Jeseon(Full-time) : building tools for zk processes and testing
- Hanjun(Full-time) : building cryptographic library for zk processes
- Dokyung(Part-time) : mathematics and cryptography verification

**Budget**

- Monthly budget
    - Full-time : $10K
    - Part-time
        - Dokyung : $3K
- Milestone 1
    - Participants : Hyung, Dongsam, Jeseon, Hanjun, Dokyung
    - Duration : 5 months
    - Budget : 5 months * $43K = $215K
- Milestone 2
    - Participants : Hyung, Dongsam, Jeseon
    - Duration : 3 months
    - Budget : 3 months * $30K = $90K
- Total budget : $215K + $90K = $305K

## Rollup Comparison : Optimistic Rollup

**The Advantage of Optimistic Rollup : Flexibility**

- Optimistic rollup can be adopted to any kind of general computation or smart contract
- Current zk-rollup technology does not provide general computation capability
    - Therefore it is only for relatively simple computation logic
    - But the technology is towards more general computation in future
    - Reason of inflexibility of zk-rollup
        - The entire computation logic should be written in circuit program, which is very low-level language without convenient functions
        - However, developers in zk-rollup industry keep supporting various kind of high-level functions from the low-level language

**Security Problems of Optimistic Rollup which can be solved by zk-rollup**

- Fraud proof assumption
    - Why problem?
        - Security depends on the existence of fraud proof activities when attack comes
        - Even with generous incentives, it is very difficult to guarantee this assumption
    - Why does zk-rollup solve it?
        - zk-rollup completely guarantees computation genuinity by very fast verifier algorithm without any need of "fisherman" like activity
        - It allows zk-rollup to possess perfect layer 1 security without any security assumption
- Delayed Finality
    - Why problem?
        - Optimistic rollup requires certain duration of time(usually 1 week) for fraud provers to find any existing fraud activities on the network
        - Even after this period, we cannot say the finality is on "layer-1-level-security", because fraud proof assumption is difficult to be guaranteed
    - Why does zk-rollup solve it?
        - zk-rollup gets its layer 1 finality via mathematical proof rather than incentivized economic activities of fruad proof
        - Any state change is 100% finalized on layer 1 when verified proof is calculated by anyone, and received by layer 1
        - Finalization latency of zk-rollup depends on the calculation time of zk-proof, but such calculation work can be efficiently parallelized, the latency is matter of minutes even in 1k tps situation
        - It can be confidently guaranteed that in near future, with enough computation resource and more efficient parallel computing algorithms, the finality latency of zk-rollup will become several seconds even in 1k tps situation

**Combination of Different Rollup Approach**

- Usecases of Optimistic Rollup
    - more generalized computation
    - less security necessity
    - less demands on fast finality
- Usecases of zk-rollup
    - more specific and concise computation
    - perfect layer-1 security necessity : utilities which need significant deposit of assets to layer 2
    - high demands on fast finality : Interchain asset transfer with fast withdrawal from layer 2

## User Benefit / Ecosystem Impact

**Efficient and Secure Scalability Solution for Cosmos Ecosystem**

- zk-rollup provides the most efficient and secure scalability solution for the Cosmos Hub and other Cosmos blockchains

**Strengthen Utility Competitiveness For Global Platform Competition**

- Especially, pegzones and other utilities with heavy deposit activities will benefit strongly
    - Applications that users have to put significant asset into layer 2 will benefit from zk-rollup support by the Hub
    - Very short finality latency without no economic assumption : dynamic asset movement across connected blockchains
    - Does not have to possess strong layer 2 security
        - because the computation consistency is verified on layer 1 before confirmation
        - it can reduce the cost of layer 2 operation by minimizing native token economics and its incentives for delegators and validators
- Scalable utilities with Hub-level security
    - Scalable utilities with fast finality on the Hub, perfect Hub-level security, various interchain connectivity will greatly benefit the users so that the Hub can attract more users from outside Cosmos Ecosystem, to possess enough competitiveness to become the mainstream utility providers for heterogenious blockchains
