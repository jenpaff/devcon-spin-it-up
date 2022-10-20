# Devcon VI Workshop - Spin it up

## 04 Demo: Exiting our validators

### Wen Exit?
- A validator can only be exited if it served a minimum amount of time 2^11 epochs (~9 days) to ensure a stable validator set
  - this is desirable due to a property in PoS called *weak subjectivity*
  - if we stop our validators for a few days and spin them back up, they would have to start from a semi-trusted checkpoint
  - the *weak subjectivity period* is the period how long a validator could stay disconnected and it strongly correlates to the rate of change in the validator set 
- Exiting a validator is also rate-limitied (validators have to wait in a queue before exiting). They can still be slashed for previously undetected misbehaviour
- A validator cannot rejoin the network after exit
- Withdrawals will only be possible with Shanghai fork
- Withdrawals will be possible after 2^8 Epochs (~27 hours)

### How Exit?
- Beacon Node (Consensus layer) is responsible for keeping track of validator status
- Send voluntary exit request to Beacon Node: https://ethereum.github.io/beacon-APIs/#/Beacon/submitPoolVoluntaryExit
- Fetch validator info (validator index, epoch to exit)
- submit voluntary exit request along with
  - validator index (which validator to exit)
  - epoch (when to exit)
  - signature (signing the message)
- Checkout an [example of an exited validator](https://prater.beaconcha.in/validator/b8c8640bdb9f50638b1be98dc60388f4051577879f9a43cf8edaeff1edc852ee2c6af6891165169564bcc9c01cef6cd8)

Further resources: 
- Implementation of [Voluntary exit](https://notes.ethereum.org/@djrtwo/Bkn3zpwxB?type=view#Voluntary-Exits)
- Short video of Justin Drake [explaining weak subjectivity](https://www.youtube.com/watch?v=iucg44ukoyI)