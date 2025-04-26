---
title: "Fermah"
seoTitle: "Fermah: Proof Marketplace"
seoDescription: "Fermah is a proof marketplace, built using AVS in Eigen Layer to provide cheap and reliable proof generation."
datePublished: Sat Apr 26 2025 03:49:58 GMT+0000 (Coordinated Universal Time)
cuid: cm9xomk3k000f09kvbxdrd2vq
slug: fermah
canonical: https://paragraph.com/@megabyte/fermah
tags: ethereum, zkp, eigenlayer

---

Zero Knowledge Proofs (ZKPs) have evolved tremendously in the past few years. There are now multiple proving systems, each with its pros and cons, and tons of use cases.

> ZK is the endgame.

With every improvement or introduction of a proving system and algorithm, there's in complexity for the software and the hardware. Every application cannot use every type of proving algorithm, and every type of hardware cannot run every proving system.

This eventually creates a new problem for generating ZKPs.

# Problem in generating Zero Knowledge Proof

Zero-knowledge proofs definitely increase the network/system's speed and throughput, but **generating proofs is a resource-intensive process** that **requires a different setup for different proving systems.**

So here there's both a demand and supply problem. And Fermah solves this problem.

# What is Fermah?

[Fermah](https://www.fermah.xyz/) is a 2-sided marketplace for proof generation.

Fermah helps in the cheap, fast and reliable generation of Zero Knowledge proofs.

Under the hood, Fermah is an Eigen Layer AVS module.

## 2-sided Marketplace

Fermah marketplace consists **Seekers** on the demand side and **Prover Nodes** on the supply side.

### Seekers (Demand)

The whitelisted users that can submit Proof Requests.

### Prover Nodes (Supply)

The nodes that generate proofs for the Proof Requests assigned to them. These prover nodes consists of Eigen Layer Operators which provide computational resources like CPU and GPU.

# How does Fermah marketplace work?

There are three major components of the Fermahs's proof marketplace:

1. Seeker
    
2. Prover Node
    
3. Matchmaker
    

![](https://storage.googleapis.com/papyrus_images/ec154d21e76ccbbbfc5f65b28ec1665b.png align="left")

Seeker first sends a request to generate proof to **Matchmaker** which is the core and is responsible for allocating requests to different Provider nodes.

The Matchmaker allocate the request based on the requirements of the proof request and maintaining competitive pricing. This orchestration ensures the optimization of the utilization rate of machines and fast generation of the proof, with the core goal to minimize the cost of generating proofs.

# Proving System supported in Fermah

The Fermah's architecture is build to support any proving system. Currently most of the major proving systems are supported and more will be add soon.

Proving systems currently supported:

* [SP1 by Succinct Labs](https://docs.succinct.xyz/)
    
* [The RISC Zero zkVM](https://dev.risczero.com/api/)
    
* [Jolt by a16z](https://jolt.a16zcrypto.com/)
    
* [Groth16 by Jens Groth](https://www.zeroknowledgeblog.com/index.php/groth16)
    
* check latest details [here](https://docs.fermah.xyz/introduction/supported-proof-systems)
    

# Current users of Fermah

The proof marketplace brought a cheap and fast way to generate zero knowledge proofs which has been a big requirement for the L2 squencers and other infrastructure components in the blockchain.

## ZKSync

ZKSync's Elastic Network helps devs by providing aggregation of the zero knowledge proofs for their app chains, and as the ecosystem expands the proof generation will cost expand proportionately. To solve this, ZKSync is partnering with Fermah for generating proofs for their sequencers.

This partnership will help ZKSync:

* Increase resilience through decentralization
    

* Lower proving costs via market competition
    
* Enhanced scalability without bottlenecks
    

## Scroll

Scroll have a complete proving pipeline where they generate different proofs (Chunk Proofs, Batch Proofs, Bundle Proofs) before submitting it to the Ethereum. This cost heavy process is now being powered by the Fermah's universal proof generation capabilities.

## Gateway

Gateway's Presto is a Platform to deploy ZK rollup in few clicks. Integration with Fermah will help all the ZK rollups to get proofs generated in a fast and reliable way.

# Who can use Fermah for proof generation?

Fermah can support any type of Proving system and it already supports major of them, therefore can network or application which requires can to generate proof can utilize Fermah proof market.

## Polygon's Agglayer

[Polygon's Agglayer](https://docs.agglayer.dev/) uses [Pessimistic Proof](https://docs.agglayer.dev/agglayer/core-concepts/pessimistic-proof/) to prove that withdrawal claim made on any chain connected to Agglayer are backed by deposits made to the unified bridge contract.

![](https://storage.googleapis.com/papyrus_images/6d436b347b823025471190ee05fb0408.png align="left")

The proofs are being generated using SP1 zkVM and the Plonky3 proving system which is supported by the Fermah marketplace.

This integration can help Agglayer in generating proof in a cost-effective manner and removing any possible bottlenecks for scalability.

# Conclusion

To conclude this post it will be right to say the infrastructure and research around ZK has been on exponential growth, and Fermah will play a critical role in generating cost-effective proof for the app chains, L2s, or basically anything that requires proof generation.

Also, more AVSs like Fermah on Eigen Layer will provide the infrastructure with shared economic security for the upcoming consumer applications in crypto.

---

Thanks for reading.

If you have any queries or feedback, [DM](https://x.com/megabyte0x)

*Keep Building (,🚀)*