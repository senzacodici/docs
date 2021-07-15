# A 51% attack explained

## Introduction

An essential strength of decentralized networks \(and blockchain technologies\) is the distributed nature of building and verifying data. Since there’s no central entity, the network’s nodes work to ensure that the protocol rules are being followed and that all network participants agree on the current state of the blockchain.

How the majority of nodes regularly reach consensus depends on the technology used by the network, but they all assume that nodes are properly distributed across the world. This means that the stakes or hash power should not be in the hands of a single entity. At least that’s not supposed to be.

In this article, we will analyze what happens when the hash rate \(or stake\) is no longer distributed properly, and one single entity or organization is able to control more than 50% of the network. The effects of a 51% attack \(also known as "majority attack"\) go beyond a simple double-spend and can destroy the whole network’s value.

## The 51% attack

The 51% attacks are critical to the security and trustworthiness of the network because a malicious actor performing such an attack would eventually become the legitimate ledger and source of truth, with the attacker’s forged records overruling other published transactions.

In this scenario, an attacker could exclude or modify the ordering of transactions, and reverse transactions they made while being in control \(leading to a double-spending\). A successful majority attack would also allow the attacker to prevent some or all transactions from being confirmed \(transaction denial of service\) or to prevent some or all other miners from mining, resulting in what is known as a mining monopoly.

Even if this attack allows double-spending, denial of service, and mining monopoly, it won’t allow the attacker to change the block’s reward, create coins out of thin air, or steal coins that never belonged to the attacker.

In the specific case of the Bitcoin network, since the consensus algorithm uses Proof of Work \(PoW\), the miners are only able to validate a new block of transactions if the network nodes collectively agree that the block hash provided by the miner is accurate. Hence, to perform a 51% attack on the Bitcoin blockchain, a single entity would have to control the majority of the hash-rate computing power. Since a miner’s performance is based on the amount of computational power he has, conducting this attack on the Bitcoin network would involve the investment of huge amounts of electricity and computational resources to achieve a 51% hash power.

## How it works

In blockchain networks such as Bitcoin, a 51% attack consists of an attacker sending a transaction to the victim, while also sending a duplicate transaction to themselves, on a private alternative blockchain fork that they are mining. The severity of the attack comes from the fact that the attacker, owning more than half the network hashing power, can create blocks at a faster rate than the rest of the network combined. As such, regardless of when they started mining an alternative blockchain fork, their chain will eventually become the rightful chain, with their alternative history overruling all of the former transactions.

In practice, a malicious actor will use this weakness to target large victims, such as crypto Exchanges. Their attack would be as follows:

1. The attacker, owning over half of the network’s hashpower, starts mining a private fork
2. Next, the attacker sends a large amount of the crypto asset to an Exchange, where they exchange it for a different currency, and withdraws it
3. Finally, the attacker publishes the private fork \(longer than the existing chain\), becoming the “real” blockchain

At this point, the attacker has the original crypto asset, as well as the currency provided by the target victim Exchange.

Even if it sounds unfeasible to achieve, it has happened in the past \(E.g. [Bitcoin Gold’s attack in 2018](https://fortune.com/2018/05/29/bitcoin-gold-hack/)\) when the network size is reasonably sized in relation to the attacker’s raw computing power.

Unlike other types of attacks, there are no simple counter measures to defend against a 51% attack other than the significant amount of resources needed by a malicious agent to reach 51% of the network’s hashpower. In that sense, we can say the bigger the network, the safer they’re against such attacks. Likewise, smaller networks can be seen as possible target candidates.

In today’s blockchain landscape, the biggest security resilience for Bitcoin’s blockchain is in fact the size and scale of their network, and the huge amount of resources that would be required to carry out an attack against it. By leveraging Bitcoin’s mining community, some smaller networks can also avoid being prey to these attacks, with the implementation of additional security and “merged mining” techniques.

Ultimately a double spend attack can not be completely prevented if the attacker has sufficient resources. If over 50% of the network is controlled by a dishonest actor then no network can survive, unless a centralized authority is in place to guarantee security. The key to strong security is how you make the resources required to perform a 51% attack as high as possible. The higher the bar is, the better the security will be.

