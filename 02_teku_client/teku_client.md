# Devcon VI Workshop - Spin it up 

## 02: Spin up your own validator with teku client

This section assumes you've walked through the previous tutorial on how to generate keys, deposit and configure and run lighthouse validator client. 

###  1. Generate keys + 2. Make a deposit

The process to generate keys and making a deposit is validator client agnostic, therefore we can walk through the same steps. For this exercise we will re-use the same key we used previously.

### 3. Setup infrastructure to run our validator

#### 3.1 Pick a validator client- this time will pick Teku

Install 

```
docker pull consensys/teku:latest
```

Test 
```
docker run consensys/teku:latest --help
```

Further installation info [follow the teku docs](https://docs.teku.consensys.net/en/latest/HowTo/Get-Started/Run-Teku/). 

#### 3.2. Configure validator client

Remember that we're taking a short-cut since we're connecting to an already synced execution client and beacon node, therefore we only need to configure our validator client. 

#### 3.3. Start validator client 
```
docker run -p 5051:5051 -v /PATH_TO_YOUR_DIRECTORY/validator/keys/:/validator/keys -v /PATH_TO_YOUR_DIRECTORY/validator/passwords/:/validator/passwords consensys/teku:latest validator-client \
    --beacon-node-api-endpoint=https://temp-hackathon:ebiu1phohv7wei2Ienuech8wath1eew8@eth-monitor-testnet.chorus.one \
    --validators-proposer-default-fee-recipient=0x000000000000000000000000000000000000dead \
    --validator-keys=/validator/keys:/validator/passwords
```

### 4. Watch your validator participate in consensus
- Go to [beaconcha.in](https://prater.beaconcha.in) and find your validator
- Look out for successful attestations 