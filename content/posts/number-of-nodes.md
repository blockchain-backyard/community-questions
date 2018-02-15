---
title: "Number of Nodes"
date: 2018-02-13T11:29:43+05:30
type: "post"
---

**Q: "Unlike Ethereum or Bitcoin, they have many nodes, so the network is safe. but Hyperledger are permissioned distributed ledgers, does that mean less nodes in this network and less security?"**

*A: Number of nodes does not impact security in Hyperledger Fabric.*

Let's start with the question. Based on the context where the question was found, Hyperledger here means Hyperledger Fabric.

I'll assume basic knowledge of Bitcoin/Ethereum and continue from there.

## Why is the number of nodes important in Bitcoin/Ethereum?

In order to simplify the whole discussion, let's boil down the tasks being done on a Bitcoin or Bitcoin like blockhain network.

1. We have new transactions that are being added to a ledger. The "security" here is that only correct transactions are being added or **WRITTEN** to the ledger.
2. We have the entire ledger available for read at any time. The "security" here is that the ledger remains consistent on future **READS**. This is about the maintainance of whatever exists so far.

In some way, we can assume that these are the only 2 "things" happening and reason about from here.

Now, regarding the number of nodes. It is clear that Bitcoin (or any Proof of Work based system) has 2 distinct types of nodes or roles of nodes. We have in Bitcoin:

1. the **Miners**: these propose new writes or updates to the system
2. the **Full nodes**: these manage the ledger and ensure that the proposed writes are consistent and that the ledger is all right.

Now, let us look at the impact of number of nodes on the system security:

### 1. the Miners

We would like more miners in the system so that .... Not very clear, right?

On this extreme, if we have no miners (and any number of full nodes for that matter), we simply will have no updates to the system. The ledger will simply remain as it is. You can read the old values, but you cannot run new transactions to **update** the ledger. Clearly that is not good.

If we have single miner, the system will start functioning. That seems alright. The miner puts togther blocks and your transactions are going through.

Why do we need more than 1 miner? We are right now completely dependent on that single aforementioned miner for writes to the ledger and for the transactions to be carried out. That is, the **single miner becomes the gatekeeper to the ledger**. Meaning, **you have to trust the miner** for your transactions to go through.

That is OK in many contexts, but in Bitcoin, the express goal is **censorship resistance**. Meaning we want the system to work in the presence of such malicous miners who may purposefully block your transactions. Meaning, the more the miners you have, the more chance you have of getting your transaction accepted.

Note that that is not fully true in the trust model that Bitcoin operates in. **You cannot really trust any of 1000 different miners, all of them may be malicious**. *Bitcoin's model of permissionless entrance allows a single entity to pose as 1000 different miners easily.* Meaning, the only way, theoretically trust a miner is if you are one.

### 2. the Full Nodes

Let us run the same analysis as above.

If there were no full node, then there is no blockchain. Trivial case.

If there is a single full node, no matter how many miners, there is a single copy of the ledger. It follows from above, that you cannot really trust that node to keep the ledger correctly. It may re-write the blockchain using a miner it controls and there would be no other trace of the truth to even argue.

It, follows that generally, having more full nodes is good. But as before the permissionless model implies that you can only trust a full node that you control.

## How is all this related to Fabric at all?

Yes, Fabric has several key differences from Bitcoin/Ethereum:

1. You run a permissioned system. Entrance to the system is gated, meaning you know who each party is in the real world. Meaning, you can always counter malicious behaviour in the system with action in the real world.
2. Fabric does not have a single global instance for cryptocurrency. A small group of participants can and do run their own system between themselves for a specific task. This task could be a crypto-currency, but more often than not, it can be anything ranging from data sharing to legal compliance.
3. All nodes are not equally trusted, you trust some nodes more than others (say your friends) and some less than others (your enemies, naturally). This reflects the real world more closely than a blanket "trust no one" policy.

Due to the above mentioned properties, you can now trust another node freely. How? Company A can trust company B, because they already trust them in the real world, they do business with them.

This is not true in general. If Company A trusts Company B completely a centralized system would be enough. More realistically, you have this spectrum of trust, for example, you trust that the company would do the right thing in the long run, but a single employee or department may be malicious.

So, herein lies the crux of the answer to the question. The number of nodes is no longer the only factor as the nodes in question are not all equal. You have a complex mix of number of nodes and the trust you place on each node.

In Fabric, you also have a range of roles from endorsing, ordering to validating/committing. Each of these roles has a different level of control on the "read/write" operations as discussed above. You choose exactly what level of trust you wish when you setup a blockchain network using Fabric. You can choose varying number of orderer nodes from different ordering models. You can choose endorsement policies based on how much you trust the endorsers. You can run any number of peers within a single organization based on how much you trust each individual peer's management.
