---
---
# Execution Layer

## Overview

* Won’t get rewards without CL
* Post-merge, it is not possible to run a consensus layer node without an execution layer node. 
* An execution layer client can be run on its own 

Execution nodes use execution client software to process transactions and smart contracts in Gnosis chains **execution layer**. 

**An execution node:** will talk to other execution nodes via peer-to-peer networking, and to a local beacon node.

There a various types of Execution Nodes

Full node: stores full blockchain data and verifies all blocks and states. All states can be regenerated from a full node.

Archive node: stores everything kept in the full node and builds an archive of historical states. As archive nodes keep the entire chain states, they are also much bigger in size than the full nodes. 
