---
---
# Consensus Layer

## Overview


The Consensus Layer handles the proof of stake consensus of the chain, which is the mechanism by which peers reach a common agreement about the present state of the network. 

**Beacon nodes**: node client software to coordinate Gnosis proof-of-stake consensus.

The beacon node tells the validator what work to do, and broadcasts the validator's work to the Ethereum network as the validator performs its duties.

**Validator** **nodes**: are responsible for proposing blocks within Gnosis chain proof-of-stake consensus mechanism, A validator will talk only to a local beacon node. 