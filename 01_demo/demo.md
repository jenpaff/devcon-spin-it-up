# Devcon VI Workshop - Spin it up 

## 01 Demo: Spin up your own validator with Lighthouse

If you want to do some more detailed upfront research, here's a [Checklist from Ethereum Staking Launchpad](https://goerli.launchpad.ethereum.org/en/checklist) to walk you through the requirements for becoming a validator.

### 1. Generate keys
- To generate your keys, download the open-sourced [staking-deposit-cli](https://github.com/ethereum/staking-deposit-cli/releases/)
- It will generate the following artefacts:
  - deposit data: which you'll need to make a deposit of 32ETH to become a validator
  - keystore file: your encrypted private signing key
- During the process the cli will ask you to 
  - pick an existing mnemonic or a new mnemonic
  - pick a decyrption password for the keystore file (encrypted version of your private signing key)
- A mnemonic (or seed phrase) is a list of 12-24 words which can act as a seed to produce a private key. The same mnemonics will always produce the same seed (deterministic!) and therefore can be used to recover keys in case of loss. If you're keen to dig in deeper into key generation find a few articles below.
- To initate key generation with a new mnemonic run `./deposit new-mnemonic`
- If it all worked out well, then it should have generated the following files:
```
➜  validator_keys tree .
├── deposit_data-1652428897.json
└── keystore-m_12381_3600_0_0_0-1652428897.json
```

Further reading on key generation: 
- General article on [Ethereum keys](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/keys/)
- Technical article on [mnemonics](https://wolovim.medium.com/ethereum-201-mnemonics-bb01a9108c38)
- Examples on how to derive [bip39 seeds and ethereum keys](https://silentcicero.gitbooks.io/pro-tips-for-ethereum-wallet-management/content/ethereum-wallet-basics/using-seed-phrases-to-create-ethereum-accounts.html)
- on (BLS Signatures)[https://eth2book.info/altair/part2/building_blocks/signatures]

### 2. Make a deposit
- Go to [staking launchpad](https://goerli.launchpad.ethereum.org/en/overview)
- Walk through the checklist
- Deposit 32 testETH
- Wait for activation

Once you've made a deposit your validator will enter the validator lifecycle which looks as follows: 
![Validator Lifecycle](https://docs.prylabs.network/assets/images/validator-lifecycle-6e7a0f5ae374d8c4eb859326055f52c4.png)

Short Cut ✂️ : Unless you're swimming in goerliETH (lucky you!) and have a lot of patience, we would suggest you take one of our pregenerated keys for the following steps which will be provided at the workshop.

Here's a [link to a more technical explanation](https://notes.ethereum.org/@hww/lifecycle#4-Validator-lifecycle) of each of the validator states.

### 3. Setup infrastructure to run our validator

#### Infrastructure involved
- Execution client (e.g. Geth/Nethermind/Besu) 
- Consensus client (e.g. Lighthouse/Teku/Prysm/Nimbus...)
  -  Beacon Node = connects to the P2P network and verifies blocks
  -  Validator client = manages validators using data from the beacon node

Short Cut ✂️: Both Execution Client & Beacon Node can take a while to sync. We will start our validator client locally and connect it to an already synced Beacon Node (provided by Chorus One)

Try connecting to our Beacon Node:

```
curl https://temp-hackaton:ebiu1phohv7wei2Ienuech8wath1eew8@eth-monitor-testnet.chorus.one/eth/v1/beacon/genesis -vk
```

If you fancy looking into the API, you can [check it out here](https://ethereum.github.io/beacon-APIs).

#### 3.1 Pick a validator client of your choice, e.g. lighthouse and install via Docker 

Install 

```
docker pull sigp/lighthouse
```

Test 
```
docker run sigp/lighthouse lighthouse --version
```

Further installation info [follow the lighthouse docs](https://lighthouse-book.sigmaprime.io/docker.html) 

#### 3.2. Configure validator client

Remember that we're taking a short-cut since we're connecting to an already synced execution client and beacon node, therefore we only need to configure our validator client. If you look at the `lighthouse_config` directory it highlights the necessary configuration.

The `validators` directory includes all signing keys that will be run by the validator client. The `validator_definitions.yaml` includes the necessary configuration e.g. path to the keystore file, decryption key, etc.

Make the following changes to configure your validator client: 
- RENAME THE FOLDER IN `validators` directory to the public key of your validator
- COPY YOUR KEYSTORE IN THE DIRECTORY
- UPDATE THE `validator_defintions.yaml`

The resulting `validator_definitions.yml` should look something like this:  

```
---
- enabled: true
  voting_public_key: "0xa5566f9ec3c6e1fdf362634ebec9ef7aceb0e460e5079714808388e5d48f4ae1e12897fed1bea951c17fa389d511e477"
  type: local_keystore
  voting_keystore_path: /home/paul/.lighthouse/validators/0xa5566f9ec3c6e1fdf362634ebec9ef7aceb0e460e5079714808388e5d48f4ae1e12897fed1bea951c17fa389d511e477/voting-keystore.json
  voting_keystore_password: myStrongpa55word123&$
```

#### 3.3. Start validator client 
```
docker run -p 9000:9000 -v /PATH_TO_DIRECTORY/lighthouse_config/:/lighthouse/ sigp/lighthouse lighthouse validator_client --init-slashing-protection --network prater --beacon-nodes "https://temp-hackathon:ebiu1phohv7wei2Ienuech8wath1eew8@eth-monitor-testnet.chorus.one" --validators-dir /lighthouse
```

### 4. Watch your validator participate in consensus
- Go to [beaconcha.in](https://prater.beaconcha.in) and find your validator
- Look out for successful attestations 

## Drawbacks of this setup 
- Keys stored locally
- Large dependency on lighthouse software 
- Lighthouse slashing protection only works for validators within the client, i.e. if we run multiple instances it won't protect us from double signing 

### Proposed solutions 
- Store keys in vault 
- Run multiple clients e.g Teku 
- Run validator clients with a remote signer 
