---
title: "Difference between Anchor and Leader peer in fabric"
date: 2018-02-16T14:06:07+05:30
type: "post"
---
**Q: "Difference between anchor peer and leader peer in fabric??"**


Every organization in Hyperledger fabric architecture can have multiple peers. When setting up a new channel, the participating organization and its associated peers, anchor peers and some other information are configured. This is done via a channel configuration transaction.

A channel configuration transaction holds the following information

1.      Participants in the channel – set of organizations

2.      Peers belonging to each participant.

3.      Anchor peers for each organization.

4.      Access Control Lists.

### An anchor peer is specific to an organization taking part in the channel and its main role is peer discovery.

It’s a peer node on a channel that all other peers can discover and communicate with. Therefore, every participating organization in a channel has an anchor peer. Peers belonging to an organization can query this peer to discover all peers belonging to other organizations in the channel.
An org can have multiple anchor peers to prevent a single point of failure.

### A Leader peer is responsible for gossip data dissemination.
 
When an ordering service node must send a block to the peers in the channel, it sends the block to each of the leading peer associated with the organizations. The leading peer in turn distributes this block to other peers belonging to the same organization. Distribution is done using gossip protocol.

Refer to this link - [Link to the project site.](http://hyperledger-fabric.readthedocs.io/en/release/channels.html)   for more information.

