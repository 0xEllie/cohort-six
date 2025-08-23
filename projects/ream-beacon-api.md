# Ream Client - A Beam client in Rust: Implement Beacon API Endpoints

Implement Beacon API Endpoints for Ream Client based on the [Beacon API](https://ethereum.github.io/beacon-APIs/) specification.

## Motivation

Ream client is an implementation of the Beam Chain specification. This project enables Ream to work with current Beacon Chain. 

Why is this needed? Key benefits:

- Beam Chain pre-launch capability:​​ Ream can sync with the Beacon Chain ​before Beam Chain goes live, ensuring readiness for the ​Beacon→Beam hard fork.
- ​ZKVM experimentation:​​ The Ream team benchmarks ​Beacon state transition functions​ running across different ZKVMs. A self-owned codebase allows easy customization to "snarkify" state transitions for Beam Chain.

## Project description

Each Beacon API endpoint has a dedicated GitHub issue tagged "Beacon API." The open issues are:

- [Implement /eth/v1/beacon/light_client/optimistic_update](https://github.com/ReamLabs/ream/issues/211)
- [Implement /eth/v1/validator/contribution_and_proofs](https://github.com/ReamLabs/ream/issues/240)
- [Implement /eth/v1/validator/sync_committee_contribution](https://github.com/ReamLabs/ream/issues/238)
- [Implement /eth/v1/validator/beacon_committee_selections](https://github.com/ReamLabs/ream/issues/237)
- [Implement /eth/v1/validator/beacon_committee_subscriptions](https://github.com/ReamLabs/ream/issues/235)
- [Implement /eth/v2/validator/aggregate_and_proofs](https://github.com/ReamLabs/ream/issues/234)
- [Implement /eth/v2/validator/aggregate_attestation](https://github.com/ReamLabs/ream/issues/233)
- [Implement /eth/v1/events](https://github.com/ReamLabs/ream/issues/227)
- [Implement /eth/v1/beacon/pool/sync_committees](https://github.com/ReamLabs/ream/issues/215)
- [Implement /eth/v2/beacon/pool/attester_slashings](https://github.com/ReamLabs/ream/issues/213)
- [Implement /eth/v2/beacon/blocks](https://github.com/ReamLabs/ream/issues/199)
- [Implement /eth/v2/beacon/blinded_blocks](https://github.com/ReamLabs/ream/issues/198)
- [Implement /eth/v1/node/health](https://github.com/ReamLabs/ream/issues/181)

Some of them may need underlying syncing & P2P layer dependencies. I'll start with endpoints ​requiring no extra dependencies, then collaborate with mentors to ​address syncing/P2P dependencies​ for remaining endpoints.

## Specification

Ream supports Pectra fork. I'll reference the specifications below.

- [Beacon API](https://ethereum.github.io/beacon-APIs/releases/v3.1.0/beacon-node-oapi.json)
- [Consensus Specs](https://github.com/ethereum/consensus-specs/tree/master/specs/electra)

We will also reference:

- [Annotated Specification of ETH2 book](https://eth2book.info/capella/part3/)
- [Lighthouse Consensus Client](https://github.com/sigp/lighthouse)

For the implementation of API endpoints, we will reference the existing Beacon API endpoints in the Ream codebase. Http handlers are located in [ream-rpc-beacon](https://github.com/ReamLabs/ream/tree/master/crates/rpc/beacon/src/handlers), which uses actix_web to manage HTTP requests and responses. API data types are defined in [ream-api-types-beacon](https://github.com/ReamLabs/ream/tree/master/crates/common/api_types/beacon/src). Additionally, the implementation utilizes ReamDB from [ream-storage](https://github.com/ReamLabs/ream/blob/master/crates/storage/src/db.rs) for data storage and retrieval.

## Roadmap

To improve my estimation for the timeline, I started working on issue [Implement /eth/v1/beacon/light_client/optimistic_update](https://github.com/ReamLabs/ream/issues/211) to see how long ​it takes​ to implement one endpoint, so I can use it as a baseline. I implemented it in one week but ran into problems testing it. After some debugging and communication with mentors back and forth, I found that it's due to a bug in the syncing functions of Ream, the latest finalized block is not being updated correctly. Based on this experience, considering Ream is still in alpha stage with active development, I expect to spend 2 - 3 weeks on each endpoint. Since other fellows and contributors will take some of the issues, I expect to finish 5-8 Beacon API issues by the 20th week of the EPF program, and wrap up the project by the 21st week. I expect to continue contributing to Ream after the EPF program.

We will try to finish all the Beacon APIs during the EPF program, below are the project milestones for me.

#### Weeks 7

- Finish [Implement /eth/v1/beacon/light_client/optimistic_update](https://github.com/ReamLabs/ream/issues/211)

#### Weeks 10

- Finish [Implement /eth/v1/beacon/pool/sync_committees](https://github.com/ReamLabs/ream/issues/215)


#### Weeks 13

- Finish [Implement /eth/v1/validator/contribution_and_proofs](https://github.com/ReamLabs/ream/issues/240)

#### Weeks 16

- Finish [Implement /eth/v1/validator/sync_committee_contribution](https://github.com/ReamLabs/ream/issues/238)

#### Weeks 19

- Finish [Implement /eth/v2/beacon/pool/attester_slashings](https://github.com/ReamLabs/ream/issues/213)

#### Weeks 20+

- Address bug fixes, draft a project report, and wrap up development.
- Explore eth2-comply (by Infura) and API test suites from clients such as Nimbus, Prysm, and Lighthouse to evaluate the possibility of automating conformance testing against the official Beacon API specifications

## Possible challenges

- Running out of Beacon API issues: Ream project is popular, other open source contributors may also take on some of the Beacon API issues. I'd be happy to work on some P2P/Networking layer issues if this happens.
- Complexity beyond my estimation: Some of the Beacon API endpoints may be much more complex than the one I used as baseline for the estimation, or some of their dependencies need heavy work. I'll communicate actively with mentors and update EPF organizers if the progress is affected.
- Beam does not currently have a formal specification. In fact, Beam has been renamed to Lean since this proposal was drafted. The scope has now also been expanded to include the Data Availability Layer and the Execution Layer. Reimplementing existing Beacon Chain features at this stage may require significant changes in the future once the Lean specifications are finalized next year.

## Goal of the project

By the end of the project. I expect all Beacon API endpoints from the above list are implemented (unless no longer needed in future versions).

## Collaborators
Since most of the Endpoints don't depend on each other, fellows & other contributors are not collaborating at the moment. I may reach out to other fellows if there are overlaps on some of the issues we're working on.

### Fellows 

[Prototype](https://github.com/ShiroObiJohn) I'm working on endpoints mentioned from above milestones. 

[Jay](https://github.com/jveer634) is a permissionless fellow who will also be working on Beacon API endpoints for the Ream client. He mentioned that his main focus will be on validator endpoints related to Sync Committees and validator registrations. We have already been in contact and plan to sync up weekly on the issues we are working on. We have also identified some shared dependencies in the P2P layer that he is currently developing.

For endpoints that depend on his work, I will use his functions or features as placeholders in my implementation. Once his work is ready, I will replace these placeholders with the actual function calls. We will follow the same practice when he is waiting for dependencies from my side.

### Mentors

Kolby ML and Kayden ML

## Resources
- [Ream Client - A Beam client in Rust: Implement Beacon API Endpoints](https://github.com/eth-protocol-fellows/cohort-six/blob/master/projects/project-ideas.md#ream-client---a-beam-client-in-rust-implement-beacon-api-endpoints)
- [Consensus Specs](https://github.com/ethereum/consensus-specs/tree/master/specs/electra)
- [Ream Client Repository](https://github.com/ReamLabs/ream)
- [Beacon API Spec](https://github.com/ethereum/beacon-APIs)
- [Annotated Beacon Chain Specification](https://eth2book.info/capella/part3/)
- [Lighthouse Consensus Client](https://github.com/sigp/lighthouse)
