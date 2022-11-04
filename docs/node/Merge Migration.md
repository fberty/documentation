---
---

# Merge Migration

for Existing Validators (pre-Merge) on Gnosischain

There are three configuration changes required for an existing set up to be merge ready for Gnosischain merge 

**1.**       **Run an Execution client** (If not already)

If you aren’t already you need to be running your own execution layer client (EL) alongside your consensus layer client (CL), Infura and alchemy will no longer work post merge.

**2.**       **Create and add JWT token**

We need to enable two-way communication between the execution layer and consensus layer, this is done via JWT secret (json web token), which is a secret key that is shared only between the two clients to authenticate one another.


**3.**       **Set Fee Recipient Address**

The _fee recipient_ is an Ethereum address nominated by a beacon chain validator to receive fees and tips from user transactions. 


## **Connecting to an execution engine- Lighthouse example**

The Lighthouse beacon node must connect to an execution engine in order to validate the transactions present in post-merge blocks. Two new flags are used to configure this connection:



* --execution-endpoint &lt;URL>: the URL of the execution engine API. Often this will be [http://localhost:8551](http://localhost:8551). This new flag replaces ‘--eth1-endpoints’
* --execution-jwt &lt;FILE>: the path to the file containing the JWT secret shared by Lighthouse and the execution engine.