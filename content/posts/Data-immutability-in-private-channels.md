---
title: "Data Immutability in Private Channels"
date: 2018-02-16T17:05:17+05:30
type : "post"
---

**Q: "How is data immutability in private channels implemented in Fabric? How can one establish which ledger stores valid history of transactions, if one party has forged data on its peer???"**

Before beginning here is some fabric terminology I’ll be using in the post (all referenced from [Here.](http://hyperledger-fabric.readthedocs.io/en/release/channels.html))

*Channel* - A Hyperledger Fabric channel is a private “subnet” of communication between two or more specific network members, for conducting private and confidential transactions.

*State Database* - The ledger’s current state data holds the latest values for all keys ever included in the chain transaction log. Since current state holds all latest key values known to the channel, it is sometimes referred to as World State. Chaincode invocations execute transactions against the current state data. Simply put, the state database is an indexed view into the chain’s transaction log, it can therefore be regenerated from the chain at any time. The state database will automatically get recovered (or generated if needed) upon peer startup, before transactions are accepted.

### The state database and the way transactions are validated in Hyperledger Fabric automatically ensures data in the peer is not forged. Let's see how.

Data (transactions) in a channel a stored in form of blocks. Each block also stores the hash of the preceding block in the chain in its header, creating a chain all the way back to the genesis block. This is the main property of any blockchain.

##Therefore, if the peer forged data in its ledger, it had to change all the blocks after the tampered block.

Let’s for the sake of argument, assume that a peer has forged its data, which means it has manipulated all the blocks starting from the first tampered block till the latest block in the chain. However other peers in the system still have a correct version of ledger. 

If the malicious peer wants to take advantage of this tampered chain, it will have to do a transaction (possibly a double spend), which will readily be invalidated because of a mismatch in the world state with other peers. This is because, a transaction submitted by the peer has to be validated at each node which is part of the channel. Validating the transaction involves checking whether a node is trying to modify the *correct version* of the key. Even if the peer manages to modify the value of a key in its own ledger, it has to match the current value of this key at other peers, which is not possible.

### To summarize, even if the peer becomes malicious and tampers the transaction log, it cannot change the world state in other peers (since every other correct peer holds a copy of the world state) and therefore cannot get away with the tampering.

Therefore, by design the peer is not motivated to tamper ledger history in the first place.

To read up more on transaction processing in Fabric, refer [here](http://hyperledger-fabric.readthedocs.io/en/release/arch-deep-dive.html)
