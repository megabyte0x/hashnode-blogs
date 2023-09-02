---
title: "A-Z of Hyperlane"
seoTitle: "Guide to Hyperlane Interoperability Protocol."
seoDescription: "A detailed guide to Hyperlane Interoperability Protocol, explaining Contracts, Mailbox, ISM, IGP, Validators, and Relayers and cross chain working."
datePublished: Sat Sep 02 2023 11:32:41 GMT+0000 (Coordinated Universal Time)
cuid: clm1y2tbh000409jmf8xq0c4b
slug: a-z-of-hyperlane
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693635122839/b05ed266-7cfa-4b6c-931d-c051f3ba7f95.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1693635104057/faea9115-de75-4616-9524-0bdec03eef0e.png
tags: blockchain, ethereum, solidity, polygon, hyperlane

---

gm gm gm!!!

Even with the vast network of blockchain, with 1000s of nodes and 100s of chains (L1, L2, L3....) the users are getting restricted in utilizing the complete potential of the decentralized network. Reason? Some application works on ETH, some on Polygon, some on Gnosis, and so on and on!

In the ever-growing network of Blockchain Networks, or ***different blockchain networks*** developers are required to build chain-agnostic applications.

> *No dependency on one chain, and no restriction for any user.*

Now solving this interoperability issue between different chains requires devs to build robust and more importantly secure designs, which is a highly technical and resourceful process, and to save devs from going through such pain [**Hyperlane**](https://www.hyperlane.xyz) comes into the picture.

In this article, we are going to learn almost all about Hyperlane and it works, so grab a coffee ☕️ and LFG 🚀

---

# What is Hyperlane?

**Hyperlane is a Permissionless Interoperability Protocol** that helps devs send arbitrary data across chains⛓

> Read in short here:
> 
> %[https://twitter.com/megabyte0x/status/1686628441888804864] 

**"Permissionless"** means *no one requires any sort of permission* from the Hyperlane Team to use Hyperlane SDK, Contracts, or even while deploying on new chains as I did👇🏻

%[https://twitter.com/megabyte0x/status/1651938319570456577] 

**"Interoperability"** means *sending between different chains*. Imagine initiating a transaction on ETH on certain to close a position on Polygon.

Hyperlane is a **complete modular protocol** that helps devs to customize it according to their application requirements.

The following are the key components of Hyperlane:

* Mailbox
    
* Interchain Security Module \[ISM\]
    
* Interchain Gas Payment \[IGP\]
    
* Agents
    

> You can either learn about all of them in deep one by one or go directly to the last section for a quick view with examples.

# Mailbox

***Mailbox is an on-chain API to send and receive interchain messages.***

> You can also check this out:
> 
> %[https://twitter.com/megabyte0x/status/1686979539291762688] 

Mailbox is the middle-man \[but CODE\] which helps devs to connect between different chains and leverage it to build something out of the box.

Their contracts are required to be present on every [chain supported by Hyperlane.](https://docs.hyperlane.xyz/docs/resources/addresses)

Mailbox Interface has two functions that are required to be present in the cross-chain contracts, those are the following:

* `dispatch`
    
* `process`
    

## dispatch

This is used to send the data from the origin chain to the destination chain.

It accepts several params:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693564023609/bc6ce931-e81f-49df-80a3-e962ee166066.png align="center")

* `_destinationDomain (uint32)`: domain for the destination chain as per the Hyperlane Docs or the deployer.
    
* `_recipient Address (bytes32)`: address of the recipient.
    
* `_messageBody (bytes)`: Raw bytes content of message body
    

It works by following these steps:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693564190795/f026d50f-6fe4-43d5-bdf6-070f91c4f8ac.png align="center")

* First, check whether the message length exceeds the 2KiB length.
    
* It formats the \_`messageBody` using [Message.sol](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/solidity/contracts/libs/Message.sol) according to the needs.
    
* [Message.sol](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/solidity/contracts/libs/Message.sol) provides a function `.id()` that returns keccak256(\_message). This \_id is saved into the ["incremental" Merkle Tree.](https://medium.com/@josephdelong/ethereum-2-0-deposit-merkle-tree-13ec8404ca4f)
    
* The Merkle Tree is used to verify fraud proofs in Hyperlane's Staking and Slashing Protocol.
    

## process

This is used on the destination chain to receive the message.

It accepts the following two params:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693564279003/2b1971f7-4392-4c37-8af9-5b7aa9374df9.png align="center")

It works by following these steps:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693593889185/09afb654-cf53-4afd-9fb4-8c843424fa5e.png align="center")

* `_message` is the formatted message from the [Message.sol](https://github.com/hyperlane-xyz/hyperlane-monorepo/blob/main/solidity/contracts/libs/Message.sol)
    
* `_metadata` is the specific Arbitrary metadata provided by the relayer used by [ISM](https://docs.hyperlane.xyz/docs/protocol/sovereign-consensus) to verify `_message`
    
* Then it checks whether this `_id` is in the array of delivered messages or not. If it is, then it reverts with **"delivered"** else add it to the array of delivered messages.
    
* Then it gets the ISM Address for the `recepientAddress`.
    
* If the recipient has defined a specific ISM it returns that else uses the default ISM for the chain.
    
* It uses the ISM address to verify messages received by the Mailbox.
    
* After all verification, it emits the message received event using the `IMessageRecepient.sol`
    

> Learn more about Mailbox [here](https://docs.hyperlane.xyz/docs/protocol/messaging).

# Interchain Security Modules

**ISM verifies the particular messages delivered on the destination chain are *actually* sent from the origin chain.**

> You can also check this out:
> 
> %[https://twitter.com/megabyte0x/status/1687471625724383232] 

ISM is used in the Mailbox, to verify the message Mailbox received from the relayer before executing txns.

Developers can define specific ISM for their applications or can use pre-built ISMs provided by the team, using the `ISpecifiesInterchainSecurityModule.sol`

ISMs are:

![Image](https://pbs.twimg.com/media/F2sYYrcbsAA_Zii?format=png&name=900x900 align="left")

<details data-node-type="hn-details-summary"><summary>Configurable</summary><div data-type="detailsContent">ISM can be configured as per the application requirements, with all the additional parameters required by the application.</div></details><details data-node-type="hn-details-summary"><summary>Composable</summary><div data-type="detailsContent">Devs can use more than one ISM in their application to increase security and work according to the consensus of the chain.</div></details><details data-node-type="hn-details-summary"><summary>Customizable</summary><div data-type="detailsContent">Devs can use IInterfaceSecurityModule.sol and can build the whole ISM according to their needs.</div></details>

Each ISM has two main components:

![Image](https://pbs.twimg.com/media/F2sYjIaboAENMUY?format=jpg&name=900x900 align="left")

## verify

![Image](https://pbs.twimg.com/media/F2sYnasaIAAJ47k?format=jpg&name=900x900 align="left")

It takes two params:

* `message(bytes)`: Message that needs to be verified.
    
* `metadata(bytes)`: Off-chain Arbitrary bytes provided by the Relayers.
    

## moduleType

It signals relayers about the type of module to be included in the metadata.

## Implementation

Implementation of a specific ISM in an application is done by using the `ISpecifiesInterchainSecurityModule.sol`

![Image](https://pbs.twimg.com/media/F2sYwiQaoAAeO6b?format=jpg&name=900x900 align="left")

There are different prebuilt ISMs provided by the team:

* MultisigISM
    
* RoutingISM
    
* AggregationISM
    
* OptimisticISM
    
* WormholeISM
    
* HookISM
    

> Learn more about ISM [here](https://docs.hyperlane.xyz/docs/protocol/sovereign-consensus).

# Interchain Gas Paymaster

***On-chain API to pay Relayer(s) to deliver the message on the destination chain.***

> You can also refer to this:
> 
> %[https://twitter.com/megabyte0x/status/1687818750635261954] 

An interchain message requires two transactions:

![Image](https://pbs.twimg.com/media/F2xWNzRboAEXST_?format=png&name=900x900 align="left")

1. Txn on the origin chain to send the message.
    
2. Txn on the destination chain to receive the message.
    

When users send the message from The **Mailbox on the Origin Chain** to the Relayer, they also need to send the gas fee amount to the Relayer to cover the fee on the Destination Chain. This gas fee is calculated with IGP and paid to the Relayer simultaneously.

![Image](https://pbs.twimg.com/media/F2xWONJbYAAw1XU?format=png&name=small align="left")

IGP Provides us with two functions:

![Image](https://pbs.twimg.com/media/F2xWOp5aIAAVYqF?format=png&name=small align="left")

* `payGasFor()`
    
* `quoteGasPayment()`
    

## payForGas

![Image](https://pbs.twimg.com/media/F2xWPEsbkAATs-U?format=jpg&name=small align="left")

It takes these params:

* `_messageId`
    
* `destinationDomain`
    
* `gasAmount`
    
* `refundAddress`
    

When the user sends the above parameters, the following are the next steps:

![Image](https://pbs.twimg.com/media/F2xWPhAbYAAoOdW?format=jpg&name=small align="left")

* It first gets the gas fee required on Destination Chain by calling `quoteGasPayment()`
    
* If the required amount is less than the `msg.value`, then go forward else revert.
    
* It calculates the overpaid amount.
    
* If the overpaid amount is more than 0, then send the amount to the `refundAddress`, else move forward.
    
* Emits Event of GasPayment.
    

## quoteGasPayment

![Image](https://pbs.twimg.com/media/F2xWQUJawAAO1bE?format=png&name=small align="left")

It takes the following parameters:

* `_destinationDomain`
    
* `_gasAmount`
    

When it receives the above parameters, it follows like this:

![Image](https://pbs.twimg.com/media/F2xWQusbYAEBM5_?format=jpg&name=small align="left")

* It first gets the token exchange rate and the gas price for the Destination Chain.
    
* Then it calculates the gas fee on the Destination Chain.
    
* At last, calculates the number of native tokens required on the Origin Chain and returns it.
    

## Complete Workflow

This is the complete **workflow of Interchain Gas Payment** on the Origin Chain to deliver the message on the Destination Chain:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693593387030/9e0f9d6d-a728-46ec-9c5d-8efe81f9253f.png align="center")

> Learn more about Interchain Gas Payment [here](https://docs.hyperlane.xyz/docs/protocol/interchain-gas-payments).

# Agents

***Off-chain actors that power Hyperlane.***

> You can also refer to this:
> 
> %[https://twitter.com/megabyte0x/status/1688514083287678977] 

There are three Agents in Hyperlane Protocol:

![Image](https://pbs.twimg.com/media/F27OnUSbEAAjnPk?format=jpg&name=900x900 align="left")

Since Hyperlane is a Permissionless Interoperability protocol, you can take the role of any agent and contribute to the security of the network.

## Validators

They sign the current Merkle root which they receive from the `Mailbox.latestCheckpoint()`

![Image](https://pbs.twimg.com/media/F27OnzdbMAEA2Os?format=jpg&name=900x900 align="left")

Validators are supposed to sign the checkpoint after it reaches the finality to maintain the security of the network.

After signing the checkpoint, validators post signatures on the available storage so that the Relayers can aggregate them.

## Relayers

Relayers are responsible for delivering messages to the recipient.

![Image](https://pbs.twimg.com/media/F27OocLaAAAXIDS?format=png&name=small align="left")

It works as follows:

1. observe Mailbox for the new messages.
    
2. Use Mailbox to get the ISM implemented by the recipient.
    
3. aggregate the metadata for the ISM.
    
4. deliver the message.
    

Relayers can also make configurations for the messages they want to deliver:

* A sender/recipient whitelist.
    
* A sender/recipient blacklist
    
* Getting paid on-chain after delivering.
    

The relayers get incentivized during the time they process the message with the gas fee.

## Watchtowers

They are the snipers in the Hyperlane protocol. They look for frauds and convicts (malicious validators) happening in the validator network and kill them by slashing the validator stake. Watchtowers are incentivized when they submit the correct report of fraud.

> Learn more about Agents [here](https://docs.hyperlane.xyz/docs/protocol/agents).

---

# Story Time

Jim wants to send an arbitrary message (data) to Dwight from ***Polygon zkEVM to Ethereum with Hyperlane.***

![Image](https://pbs.twimg.com/media/F3AXEMZb0AEz0Df?format=jpg&name=medium align="left")

> You can also refer to this:
> 
> %[https://twitter.com/megabyte0x/status/1688875226745253888?s=20] 

The following steps will be required to transfer the data:

![Image](https://pbs.twimg.com/media/F3AXE5EbkAAfJJA?format=jpg&name=medium align="left")

1. Jim sends the `message data`, `recipient address`, and `chain` where the recipient is to the **Polygon zkEVM Mailbox contract.**
    
2. **Mailbox** **emits an event** that gets indexed.
    
3. **Validators** **sign** the latest checkpoint.
    
4. The **signature gets stored in the** **database**.
    
5. During the above process, Jim **pays the gas fee using** **IGP**.
    
6. When the gas fee is transferred to the **Relayer**.
    
7. The **relayer sends the message and metadata** on the ETH Mainnet.
    
8. The `messageBody` sent to **ISM** **to get verified.**
    
9. The message gets **delivered to Dwight on the ETH Mainnet**.
    

---

> Whoo!!🤩 This was quite a lot of workflow and solidity contracts understanding.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">If you wanna explore more check out the <a target="_blank" rel="noopener noreferrer nofollow" href="https://docs.hyperlane.xyz" style="pointer-events: none">Hyperlane Docs</a>, and if you have any queries get them resolved in <a target="_blank" rel="noopener noreferrer nofollow" href="http://t.me/c/1264208834/1" style="pointer-events: none">Hyperlane India Telegram Channel.</a></div>
</div>

You can connect with me on [Twitter](https://twitter.com/megabyte0x), [Lenster](https://lenster.xyz/u/megabyte0x) or [LinkedIn](https://linkedin.com/in/megabyte0x).

Happy Learning 🙌🏻

Keep Building 🧱🚀