# Devcon VI Workshop - Spin it up 

## 03 Spin up your validators using a remote signer

This section assumes you've completed the previous tutorials which include more detailed description on how to setup and conigure the consensus clients. Further, this part will only walk you through the concepts and provide resources in case you'd like to try the setup yourself.

### Slashing & the risk of double-signing
Validators have the following responsibilities:
- validating new blocks and making attestations
- proposing new blocks when selected at random from the validator pool

To disincentive misbehaviour, validators carry the risk of getting slashed (i.e. having ETH deducted from their deposit). Slashing is a mechanism built into blockchain protocols to discourage validator misbehavior and incentivize security, availability, and network participation.

#### Slashing 
**Three ways of getting slashed:**
1. By proposing and signing two different blocks for the same slot
2. By attesting to a block that "surrounds" another one (effectively changing history)
3. By "double voting" by attesting to two candidates for the same block

Double signing occurs when a validator (private key) submits two signed messages for the same block. This can happen if a node operator or infrastructure provider optimizes their node configuration to prevent downtime by having a highly available backup entity running at the same time as a primary entity.

Further resources:
- Link to documentation on [rewards & penalities](https://ethereum.org/es/developers/docs/consensus-mechanisms/pos/rewards-and-penalties/)
- Summary on [slashing and other penalites](https://www.blocknative.com/blog/an-ethereum-stakers-guide-to-slashing-other-penalties)

### Using a remote signer
e.g. [web3signer](https://docs.web3signer.consensys.net/en/latest/)
- provides a slashing protection database which keeps track of each block and attestation signed by a validator.


### Infrastructure involved:
- Execution client (e.g. Geth) 
- Consensus client (e.g. Lighthouse/Teku/Prysm/Nimbus...) = Validator client + Beacon Node
- Remote Signer (e.g. web3signer) including a global slashing database

**NOTE**: Both Execution Client & Beacon Node can take a while to sync [..INSERT MORE INFO...]

### Steps to setup validator clients with web3signer
1. Setup global slashing database
2. Start web3signer connecting to the global slashing database
3. Configure lighthouse with web3signer & start client
4. Configure teku with web3signer & start client