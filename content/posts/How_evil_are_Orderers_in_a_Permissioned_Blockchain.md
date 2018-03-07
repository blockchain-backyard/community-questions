---
title: "How evil are Orderers in a Permissioned Blockchain"
date: 2018-03-07T12:22:27+05:30
type : "post"
---


Original Question : 

## The "orderer" is seen as an issue with respect to confidential data. Can someone briefly tell me why? In case: Creating multiple orderers and having a random selection of one of orderer by the client Would it mitigate the problem?

Let us first understand the role of an Orderer. In case of Hyperledger fabric, the _Orderers_, aka the **Ordering Service Nodes** provide 

a) a shared communication [channel](http://hyperledger-fabric.readthedocs.io/en/release/channels.html) to the clients and peers 
b) delivery guarantees like total-order broadcast, atomic broadcast, and consensus).

The clients can send [endorsed transactions](http://hyperledger-fabric.readthedocs.io/en/release/txflow.html) to the OSN (via a peer), which guarantees total order and reliability in the communication of these transactions. This makes sure that all peer nodes have the same messages in the same atomic order, given them and the OSN are part of the same channel. 
When a transaction reaches an orderer, it is bundled into a block along with other messages it has received from other peers based on a logical ordering. This implies that an orderer can see the contents of a transaction (a lot of information including the transaction payload, readset, and writeset), which can be considered a privacy concern. 

## Is there a way to preserve the privacy of these transactions?

**Currently, NO.**

However, Hyperledger fabric is private and permissioned, meaning-
a) Identity of all participants are known
b) There are policies on which nodes can join a particular channel.
The OSN is a node trusted by an organization, and it sees information relevant for conducting business with the organization. 

Coming to the second part of the question- 

## If there were multiple Orderers and the client randomly chose one Orderer?

Well, one of the delivery guarantees of the Ordering service is consensus, meaning, all the Ordering nodes must have the same blocks and the transactions inside them must be in the same order. To achieve that, transactions must be present at ALL Orderers (even the ones belonging to the other organizations who are part of the channel?), failing which there could be an inconsistency in the global state.  Therefore randomly selecting an Orderer, which in itself would over-complicate the ordering process, would not really help. Even if one found a way to hide the tx from other Orderers, this one Orderer is still able to view the data and thus exposes the privacy concern.

# What if the Orderer, having access to all data in the channel, decides to manipulate it?

Unless there is a single orderer, there needs to be a consensus in the data between all the orderers. But for argument’s sake, if manipulated blocks from one orderer reach a set of peers, they will not blindly trust the contents received. Responses from all the endorsing peers (who had initially endorsed the transaction inside a block) are checked and the current world state of the peer is also considered. Therefore, the tampered block will get invalidated by the peer during one of these checks. 

# And, what if the peer is unable to detect a faulty block and applies the incorrect state update?

In this case, the peer(s) would have diverged from the world state maintained in other honest nodes in the network, hence all transactions at this peer will start getting invalidated. Hence the entire task of tampering the transactions at the orderer becomes meaningless. 

**Takeaway**

1.    The Orderer is a trusted node belonging to an organization and it operates in a permissioned environment. The trust model is such that visibility of data at the Orderer is not much of an issue.

2.    Even though the Orderer can _see_ the data, it is _incentivized not to tamper with it_. 


