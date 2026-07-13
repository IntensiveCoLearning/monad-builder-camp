---
timezone: UTC+8
---

# christina246543

**GitHub ID:** christina246543

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
The incentive mechanism design incorporates game theory principles. Controlling inflation and maintaining scarcity through mechanisms like Bitcoin's halving events creates deflationary pressure that supports long-term price appreciation. The system is designed to incentivize long-term holders, and the reduction in mining rewards aligns with deflationary expectations, potentially driving prices upward. The design also encourages miner and validator participation—rewards must be sufficiently attractive to sustain network security and operational stability. Balancing these economic incentives with decentralization and security remains the core challenge in blockchain mechanism design.
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->

Hard forks represent an important governance phenomenon in blockchain ecosystems. When a large portion of the community supports a protocol modification, a hard fork can occur, resulting in two separate chains. Under Proof-of-Work mechanisms, a hard fork supported by over 51% of computing power is considered the legitimate main chain. The key difference between Bitcoin and Ethereum forks is their classification: Bitcoin has undergone forks such as BCH and BSV, while Ethereum's most notable fork was the DAO incident, which split the chain into the current Ethereum (ETH) and Ethereum Classic (ETC).
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->


Blockchain scalability remains a critical challenge. Block size directly impacts transaction throughput—larger blocks can handle more transactions per batch. Layer 2 scaling solutions represent a major architectural approach to significantly improve TPS (transactions per second). The number and distribution of nodes directly affect the degree of decentralization; higher node count and broader geographic distribution correspond to stronger decentralization.
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->



This study summary provides a comprehensive overview of blockchain operating mechanisms, covering distributed network architecture, decentralization principles, incentive systems, scalability solutions, and governance dynamics.

Blockchain operates on a distributed network architecture where numerous nodes, also referred to as miners, collectively maintain and record transaction data. Each node stores a complete, identical copy of the blockchain ledger, which is the foundational characteristic that distinguishes blockchain from traditional centralized systems. This distributed structure gives rise to two core new features: decentralization and true immutability.

Regarding decentralization, blockchain networks are distributed globally, with each node maintaining a full copy of all data. As long as at least one node remains operational, the blockchain data persists indefinitely. True immutability stems from the global distribution of nodes—no single entity controls the majority of nodes. Since altering records requires control of more than 51% of nodes, which is practically infeasible across a globally distributed network, the data becomes effectively tamper-proof. However, it is worth noting that early Bitcoin networks had relatively few nodes, meaning decentralization was not as pronounced in the early stages. The degree of decentralization also varies across different blockchain networks. For instance, if nodes are concentrated in just a few countries or controlled by a small number of large mining pools or exchanges, the network becomes more centralized and more vulnerable to manipulation.

Nodes that participate in bookkeeping and provide network services receive rewards. These network service providers are compensated with different tokens depending on the specific blockchain network they support. The rewards possess monetary attributes—designed based on comparison with fiat currencies, they function as a form of currency with transferability and monetary value. Bitcoin's total supply is capped at 21 million, and its price is determined purely by market supply and demand dynamics. The MicroStrategy company's active purchases of Bitcoin have contributed to reduced market volatility and increased demand.

The reward quantity is established by protocol rules—for example, Bitcoin's block reward is currently 3.125 BTC and follows a halving mechanism. In terms of market value, the reward's worth equals the token price multiplied by the reward amount, and miners or validators balance operational costs against revenue to determine participation levels. Different blockchain networks employ distinct reward mechanisms. Bitcoin originally used Proof-of-Work with mining rewards, while Ethereum has transitioned to Proof-of-Stake with staking rewards, where the ETH issuance rate adjusts dynamically based on total staked amount. Many Proof-of-Stake chains use fixed annualized inflation rates, and some new chains incorporate gradual token unlocking to attract early participants and sustain long-term growth.

A decentralized network and blockchain require continuous token incentives to maintain operation. Miners, who serve as network service providers, invest in mining machines and electricity costs to perform transaction verification and bookkeeping. They receive token rewards as compensation for their work. Gas fees are another incentive mechanism—users pay gas fees when initiating transactions, and miners earn these fees by processing transactions. If transaction fees are insufficient to cover operational costs, miners may reduce their participation. User-initiated transactions flow through wallet applications, and smart contracts automatically execute the operations.

Blockchain scalability remains a critical challenge. Block size directly impacts transaction throughput—larger blocks can handle more transactions per batch. Layer 2 scaling solutions represent a major architectural approach to significantly improve TPS (transactions per second). The number and distribution of nodes directly affect the degree of decentralization; higher node count and broader geographic distribution correspond to stronger decentralization.

Hard forks represent an important governance phenomenon in blockchain ecosystems. When a large portion of the community supports a protocol modification, a hard fork can occur, resulting in two separate chains. Under Proof-of-Work mechanisms, a hard fork supported by over 51% of computing power is considered the legitimate main chain. The key difference between Bitcoin and Ethereum forks is their classification: Bitcoin has undergone forks such as BCH and BSV, while Ethereum's most notable fork was the DAO incident, which split the chain into the current Ethereum (ETH) and Ethereum Classic (ETC).

From a decentralization perspective, Bitcoin is generally recognized as the most decentralized network, with tens of thousands of nodes worldwide and no single controlling entity. In contrast, newly launched PoS public chains tend to have lower decentralization, as early-stage token distribution is often concentrated among founding teams and early investors. In theory, well-established PoS chains can achieve decentralization comparable to PoW chains over time. Regarding the most centralized public chains, some argue that certain DAO-governed chains like ETH remain relatively decentralized, while others point to Solana as an example of lower decentralization due to its concentration among large validators.

The incentive mechanism design incorporates game theory principles. Controlling inflation and maintaining scarcity through mechanisms like Bitcoin's halving events creates deflationary pressure that supports long-term price appreciation. The system is designed to incentivize long-term holders, and the reduction in mining rewards aligns with deflationary expectations, potentially driving prices upward. The design also encourages miner and validator participation—rewards must be sufficiently attractive to sustain network security and operational stability. Balancing these economic incentives with decentralization and security remains the core challenge in blockchain mechanism design.
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




Blockchain is a decentralized distributed ledger technology that securely, transparently and immutably records transaction data across network nodes. Its core unit the block consists of transaction records and the hash summary of the previous block, with limited storage capacity that shapes network operation. Larger block sizes raise transaction throughput but increase node storage costs and weaken decentralization, while smaller blocks limit processing efficiency. Blocks are bundled at fixed intervals by miners for accounting. Shorter block creation times speed up transaction confirmation yet raise orphan block risks, and longer intervals boost network security by reducing temporary forks. Linked sequentially via each prior block’s hash value, blockchain data is tamper resistant. Altering any historical block requires rewriting all subsequent blocks. Special chain adjustments include rollbacks which are rare coordinated reversions of confirmed blocks amid severe attacks, hard forks which are non backward compatible protocol upgrades splitting the chain permanently such as Bitcoin Cash, and soft forks which are backward compatible updates where unupgraded nodes still validate basic transactions like Bitcoin’s SegWit upgrade. As core blockchain traits, full data transparency pairs with wallet anonymity. Privacy chains such as Monero and Zcash use zero knowledge proofs and ring signatures to hide sender, receiver and amount information, trading partial transparency for advanced privacy without conflicting with public blockchains as they serve distinct user demands. Blockchains enable fast borderless settlements far quicker than traditional cross border remittances, yet transaction speeds vary drastically. Bitcoin hits roughly seven transactions per second, Ethereum fifteen to thirty transactions per second, and new chains like Solana reach tens of thousands transactions per second. This throughput gap is determined by four key factors. They are consensus mechanisms where proof of stake outperforms proof of work, block generation intervals, block storage capacity, and network expansion architectures including sharding and Layer 2 solutions.
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->





A blockchain wallet serves as the management tool for private keys rather than a storage container for digital assets. A public wallet address is converted and derived from the unique private key, available for public transfer inquiries and asset receiving. The private key fully represents asset ownership and identity authority, which must be kept confidential entirely. Digital signatures created via private keys verify the real initiator of on-chain behavior, achieving identity authentication without disclosing private key information to the public network.
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
