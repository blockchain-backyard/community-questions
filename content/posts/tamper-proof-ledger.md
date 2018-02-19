---
title: "How 'tamper-proofed' is the ledger in Hyperledger Fabric?"
date: 2018-02-19T12:01:13+05:30
type: "post"
---

**Q: Especially in features such as the "channel" in Hyperledger Fabric, where only parties involved in a set of deals share a common ledger, isn't the ledger not really tamper-proof?**

To begin with an overarching answer, none of the existing blockchain technologies are really tamper-proof.
Even with Bitcoin, which sparked the whole discussion on tamper-proofed shared ledgers, it is only computationally hard (read, *very very hard*) to tamper with the ledger.
However if all the miners did agree to rewrite the ledger, it would most certainly be possible to do so<sup>1</sup>.
The reason we are not likely to see this happen is that it is most unlikely that all the miners would agree on a new ledger state (consensus is a *HARD* problem).
Thus, while theoretically Bitcoin is not tamper-proof, in practice it indeed is. 

Similarly, even with Hyperledger Fabric, the ledger is not theoretically tamper-proof since the participants could collude to tamper with the ledger, or even rewrite it entirely.
However, the question of whether the ledger is tamper-proof in practice depends on what our adversary model is. For our discussion, we will consider the following two adversary models:

1. **A participant or a subgroup of participants in the channel**
2. **All the channel members as externally visible to an auditor**

## The first adversary model
In the first adversary model, we assume that a subset of the participants in the channel are interested in tampering with the ledger. For addressing this we will assume that the adversarial participants are a minority where by minority we imply that the adversaries cannot subvert the system<sup>2</sup>.

In Hyperledger Fabric, all the state updates are governed by pre-defined smart contracts.
The only means of changing state is by executing these smart contracts which happens through transactions that invoke the required smart contracts.
These transactions are allowed to proceed only if they have the necessary endorsements as specified in the endorsement policy for the chaincode.
Presumably, the endorsement policies for the application were defined correctly in the sense that the satisfaction of the policy implies a consent to the transaction from all the participants<sup>3</sup>.
Therefore, there is no way that the adversaries can tamper with the state as reflected by the shared ledger since the Fabric transaction logic will not allow such updates to be processed as the corresponding transactions would not be endorsed by the honest participants. 

**Are there other ways for the adversaries to tamper with the data outside the Fabric transaction flow?** <br>
The ledger, and the associated state, in a Fabric channel is maintained individually by each participant in the channel (at the associated peer).
Since each peer owns its own copy of the ledger, it can go ahead and modify its state locally.
Adversaries can therefore effect a tampering by modifying their local copies. 
However, this is what I call tampering the data '**Ostrich-style**'.
Just because an adversary(s) changes his view of the state it does not imply all the participants in the channel would also view the state differently.
The other honest participants would continue to see only the legitimate updates to the state. 
Since the adversaries are a minority by our assumption, there is no way that they can convince others that theirs is the correct state. 
Hence, again, no tampering.

## The second adversary model
In the second model, we are assuming that there is an external auditor who requires access to the transactions between a group of participants. For example, a government body could be interested in looking at the transactions between a group of financial institutions in the event of a fraud. The implicit assumption here is of course that the financial institutions were transacting through a Hyperledger Fabric network. 

Of course, as discussed earlier, if all the participants agree on rewriting the ledger to cover up the fraud, they most certainly can do so. 
They can follow the protocol all the way and yet achieve the rewriting by submitting new transactions from the exact point they want the history to be rewritten.
While this would take some time subject to the performance characteristics of the underlying Fabric network, it is not computationally hard as in the case of Bitcoin. 
So what is the solution for this case? There are two ways to achieve tamper-proofing (or detecting tampering depending upon the solution used) here:

1. **The auditor should be a member of the channel** and track all the activities.
Of course, the tamper-proofing is achieved only from the point that the auditor joins the channel.
The history prior to the auditor joining could have been tampered with by the participants.
However, once the auditor joins, it can track any tampering including with the history as available with the auditor from the time of its joining.
Depending on the power that is given to the auditor, it can also force the channel participants to sync with its own copy in the case of divergence of state between the auditor and the participants.
2. Another solution to this would be to require that **a hash of the ledger state be periodically published in an external forum**.
This enables the auditor to detect tampering since any tampering would lead to a change in the hash of the state and a consequent mismatch with the published hash.
Of course, the auditor cannot in this case know what the original data was.
Also, the strength of this solution is tied to how tamper-proof the external forum itself is.
A good external forum in this regard is the ongoing Bitcoin chain itself.
Since it is computationally hard to rewrite the Bitcoin history, we can assume safely that the published hash can never be tampered with. 

## Conclusion
As discussed, the ledger in Hyperledger Fabric is definitely not tamper-proof theoretically.
The answer to whether in practice it is tamper-proof depends on the adversary model.
However, as elucidated, there is always a means to make the ledger tamper-proof. 
Therefore, we can safely conclude that the ledger for a channel in Hyperledger Fabric is in fact tamper-proof in practice. 

<br>
**Disclaimer**: <br>
Some of the statements that we have made may not entirely be technically correct. They were however made to keep out the gory details otherwise needed to support a technically correct answer. We do provide a greater level of detail through the footnotes below. 


<br> 
**Footnotes:** <br>
<sub> 1. Requiring all the miners to agree is an exaggeration. All you need is in fact a majority in terms of computation power in the network to achieve this.</sub> <br>
<sub> 2. The exact requirements for them to be a minority would depend on how consensus on the state is arrived at when the ledgers for different participants are out of sync. Discussion on this is beyond the scope of this article.  </sub> <br>
<sub> 3. The statement that all the participants in the channel provide consent to the transaction when in fact the endorsers for the transaction might be a subset of the participants might seem confusing.
However, the endorsement policies for a smart contract in a way define ownership of the data associated with the smart contract.
So if you are not an endorser it implies that you are only an observer of the transaction and your consent to the transaction is implicit.
And if you are an endorser who were against the transaction, it implies that for that particular transaction you have been forced to be an observer since other endorsers have the power to overrule your judgement.</sub>

