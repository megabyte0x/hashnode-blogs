---
title: "Building a TokenSwap Subgraph"
seoTitle: "Deploy Subgraphs for Token Swap Contract"
seoDescription: "Learn to build and deploy subgraphs with The Graph Protocol for token swaps on Polygon. This guide covers Solidity, GraphQL, and more."
datePublished: Fri Oct 27 2023 05:52:50 GMT+0000 (Coordinated Universal Time)
cuid: clo875lt000170al3fgar69qa
slug: building-token-swap-subgraph
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1694020968182/ad8d2e56-b8ac-4427-964a-15f3c2904756.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1694021013304/9dcd4c07-2e56-4f20-ab5c-321bab993fb3.png
tags: graphql, blockchain, ethereum, solidity, subgraph

---

gm gm gm!!!

*Indexing Blockchain Data is really really hard* and indexing transactions of something like [Uniswap](https://uniswap.org/) is a lethal task. But [**The Graph Protocol**](https://thegraph.com/) greatly reduces the complexity of this.

We will be learning how to build, deploy, and play with Subgraph for a simple Token Swapping Contract.

The following are the prerequisites for this tutorial:

* Basic Solidity and GraphQL understanding.
    
* [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/)
    
* [Remix](https://remix.ethereum.org/)
    
* [VS Code](https://code.visualstudio.com/download)
    

---

# Building the Token Swap Contract

This is the Token Swap Contract we will be using for our example.

%[https://gist.github.com/megabyte0x/2877d16843008371ff19cc93a3ac778d] 

The following are the main functions we will be focusing on in our subgraph:

* `addLiquidity(uint256 _tokenAmount)`: This adds liquidity to the pool and emits `GraphKitExchange__LiquidityAdded` event.
    
* `removeLiquidity(uint256 _amount)`: This removes liquidity from the pool and emits `GraphKitExchange__LiquidityRemoved` event.
    
* `ethToTokenSwap(uint256 _minTokens)`: This swaps ETH to the Token and emits `GraphKitExchange__ETHToTokenSwap` event.
    
* `tokenToETHSwap(uint256 tokenSold, uint256 minTokens)`: This swaps Token to ETH and emits `GraphKitExchange__TokenToETHSwap` event.
    

Having these above events is important as the Graph Nodes will be listening to these events and storing the data (defined in schema) as it gets emitted.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">To learn more about Events in Solidity, check <a target="_blank" rel="noopener noreferrer nofollow" href="https://www.youtube.com/watch?v=sas02qSFZ74&amp;t=12120s" style="pointer-events: none">this</a> out.</div>
</div>

This is the Basic ERC20 Contract we will be using:

%[https://gist.github.com/megabyte0x/1cd407947d578059235287cc275f5f0d] 

---

# Deploy the Contracts

Now deploy these contracts using Remix on Polygon Munbai Testnet.

<div data-node-type="callout">
<div data-node-type="callout-emoji">⚠</div>
<div data-node-type="callout-text">Make sure to deploy <code>Token.sol</code> first and then use that contract address and deploy <code>GraphKitExchange.sol</code></div>
</div>

Head to your [Remix IDE](https://remix.ethereum.org/) and follow the steps:

1. Connect with the Injected Provider and select Polygon Mumbai Testnet as the network.
    
2. Get some **TEST MATIC** on Mumbai Testnet from the Faucet, [here](https://faucet.polygon.technology/).
    
3. Deploy `Token.sol`
    
    1. Copy the above contract and paste it into the Remix file.
        
    2. Then select the contract and enter the constructor parameters. I have entered `MEGABYTE`,`MB` and `100000000000000000000`
        
    3. Hit Transact.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693137850817/9aef89e0-3162-4dd8-897c-86a3d5f1088f.png align="center")
    
4. Copy the above GraphKitExchange code and paste the content of `GraphKitExchange.sol` in the Remix File.
    
5. Copy the `Token.sol` Contract Address
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693135922129/b1d0c93c-d3b8-4c3f-821a-52fcd49e70b6.png align="center")
    
6. Select the GraphKitExchange.sol, paste the Token Contract Address, and hit Transact.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693135989441/6c457f82-1f84-4cc0-adf9-443878d6edcc.png align="center")
    
7. Verify the contracts on the [Mumbai Scan](https://mumbai.polygonscan.com/)
    
    > Learn how to do it [here](https://www.quicknode.com/guides/ethereum-development/smart-contracts/different-ways-to-verify-smart-contract-code#method-3-verify-via-remixide--etherscan-plug-in).
    
8. Keep both the contract addresses handy, we will be using them shortly.
    
    > Here are mine though:  
    > Token Contract: [0xB2887fA56b2601cB5877A99188c3f23731F671F5](https://mumbai.polygonscan.com/address/0xB2887fA56b2601cB5877A99188c3f23731F671F5)
    > 
    > Exchange Contract: [0xd8F178e60F3A7434F0F4E2519e6C2B89575f8123](https://mumbai.polygonscan.com/address/0xd8F178e60F3A7434F0F4E2519e6C2B89575f8123)
    

---

> Congrats your contracts are ready 🔥
> 
> Let's deploy the Subgraph for the same 🚀

---

# Build the Subgraph

To build the Subgraph, these are the steps that need to be followed:

* Create a Subgraph on Subgraph Studio.
    
* Install the Graph Protocol CLI
    
* Initialize your Subgraph⁠
    
* Deploying a Subgraph to Subgraph Studio
    

> Before building the subgraph, if you wanna revisit the concepts of Subgraph check this out 👇🏻
> 
> %[https://twitter.com/megabyte0x/status/1689177201147805696] 

## Create a subgraph on Subgraph Studio

1. Head to [https://thegraph.com/studio](https://thegraph.com/studio)
    
2. Create Subgraph on Polygon Mumbai Testnet
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693137682942/bbf55fac-e7f6-410b-a582-0d985eb926a7.png align="center")

1. Keep the **Subgraph Slug** and **Deploy Key** handy.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693138083744/3a972c77-9ccf-46bb-b692-0bf9e459e9fd.png align="center")

## Install the Graph Protocol CLI

You need to install `graph-cli` to use the Subgraph Studio from your system.

Follow the steps for installing it:

1. Open up your terminal.
    
2. Paste the following command and hit ENTER:
    
    ```bash
    yarn global add @graphprotocol/graph-cli
    ```
    

## Initialize your Subgraph⁠

It's time to start working on your Subgraph:

1. Create a new directory.
    
    `mkdir tokenSwapSubgraph`
    
2. Initialize the subgraph \[Paste your subgraph slug\]
    
    `graph init --studio <SUBGRAPH_SLUG>`
    
3. Choose `ethereum` as the protocol
    
4. Enter the subgraph slug name
    
5. Name the directory.
    
6. Select `mumbai` as the Ethereum Network
    
7. Enter the Contract Address for `GraphKitExchange.sol` contract.
    
8. Use the default start block.
    
9. Enter the name of the contract, here is `GraphKitExchange`
    
10. Select `true` for **the index contract event as entities**
    
11. Type `y` for entering a new contract address (for `Token.sol`)
    
12. Type `n` for providing local ABI path.
    
13. Enter the name of the contract, here is `Token`
    

Here's how it will look in your terminal 👇🏻

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693141891831/6ca51d7f-3321-4607-bdbf-92364cc70240.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693141908775/4f10f8a1-a851-4e36-bb9e-8dd444e4022c.png align="center")

1. Go to your subgraph directory
    
    `cd graphkitexchange`
    
    In the directory, you can see the following files:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693143605841/d1d09396-6929-407c-bcaa-362600e52d76.png align="center")
    
    In the `src` directory you will see that it auto-generated the mappings for your subgraph as per the events in your contracts.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693143672187/e5cc4fa5-885f-4423-9138-b162c34c1d2c.png align="center")
    

> Your Subgraph is ready now. It's time to deploy it.

## Deploy the Subgraph

We are almost done! Let's deploy our subgraph.

1. Authenticate your Graph Studio Key \[Get Deploy Key from your subgraph studio\]
    
    ```bash
    graph auth --studio <DEPLOY KEY>
    ```
    
2. Deploy your subgraph with your `subgraph slug`
    
    ```bash
    graph deploy --studio graphkitexchange
    ```
    
3. Enter the `v0.0.1` as the version for your subgraph
    
4. In the end, you will see this 👇🏻
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693143955284/5daa5f9a-a7b1-41bb-a756-4a0abf24ceac.png align="center")
    
    Here you got the <mark>subgraph endpoint, </mark> [https://api.studio.thegraph.com/query/48418/graphkitexchange/v0.0.1](https://api.studio.thegraph.com/query/48418/graphkitexchange/v0.0.1)
    

> Whoo!!! That's a lot of work! But congrats 🤩 You deployed your subgraph for a Token Swapping Contract.

# Play with the Subgraph using Subgraph Studio

So, it's time to use a decentralized, most effective and inexpensive way of querying the blockchain data.

I'll be using a Subgraph Playground for the same. \[Not a frontend dev... hehe\]

## Creating Txn on the contracts

Before using the subgraph we need to create some transactions so we can query them.

Follow the steps to approve `GraphKitExchange`'s contract address on `Token` Contract.

1. Open your `Token.sol` contract on Mumbai Scan, and head towards the `Contract` Tab. [*Here's mine.*](https://mumbai.polygonscan.com/address/0xB2887fA56b2601cB5877A99188c3f23731F671F5#code)
    
2. Click on the `Write` tab below `Contract` tab. *Since you have already verified the contract, you will be able to Write transactions on it using the Etherscan UI.*
    
3. Connect your Metamask with the Etherscan by clicking on `Connect to Web3`
    
4. Now head to the `approve` function and open the modal.
    
5. Enter the address of the `GraphKitExchange` contract address and the amount in `wei`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693510431413/53f78846-0f4d-4489-b0ea-130d57be35ea.png align="center")
    
6. Click on `Write` and confirm the transaction.
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">😀</div>
<div data-node-type="callout-text">Since we have approved <strong>5 MB </strong>to <strong>GraphKitExchange Contract</strong> it's time to add some Liquidity into the contract.</div>
</div>

Follow these steps to create your first transaction on the `GraphKitExchange` Contract:

1. Open your `GraphKitExchange.sol` contract on Mumbai Scan, and head towards the `Contract` Tab. [*Here's mine.*](https://mumbai.polygonscan.com/address/0xd8F178e60F3A7434F0F4E2519e6C2B89575f8123#code)
    
2. Click on the `Write` tab below `Contract` tab. *Since you have already verified the contract, you will be able to Write transactions on it using the Etherscan UI.*
    
3. Connect your Metamask with the Etherscan by clicking on `Connect to Web3`
    
4. Click on `addLiquidity` and enter the 5 Matic and 5 MB (in wei) into the respective fields.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693510952482/66c8aba4-0104-42fd-9fc9-797b4d8ae11f.png align="center")
    
5. Click on `Write` and confirm the transaction.
    
6. You will see a `View Transaction` button, click on it and copy the `transaction hash`
    

> Voila! We are good to go with our subgraph.

## Querying the Subgraph

Now we have a transaction ready in your GraphKitExchange Contract waiting to be queried by us.

Let's do this, by following these steps:

1. Head to your deployed Subgraph and select the playground. Mine is [https://thegraph.com/studio/subgraph/graphkitexchange/playground](https://thegraph.com/studio/subgraph/graphkitexchange/playground)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693509839817/5d64bd1f-2b70-45ba-9e24-f540da489a88.png align="center")
    
2. Now paste this into the query
    
    ```graphql
    query MyQuery {
      graphKitExchangeLiquidityAddeds 
    (where: {transactionHash: "Your Transaction Hash"}){
        _ethAmount
        _lpAmount
        _tokenAmount
        _user
      }
    }
    ```
    
    <div data-node-type="callout">
    <div data-node-type="callout-emoji">💢</div>
    <div data-node-type="callout-text">In "Your Transaction Hash" be sensible and paste your transaction hash 😂</div>
    </div>
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693511663832/f72e3998-0962-41e8-b148-7585a9f752fa.png align="center")
    
3. Now click on that **YOUTUBE PLAY BUTTON**
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693511706315/8847b6bb-f25b-42a3-979f-cf27dfa6c18b.png align="center")
    
    As you can see on the right side we got this 👇🏻
    
    ```json
    {
      "data": {
        "graphKitExchangeLiquidityAddeds": 
          [{
            "_ethAmount": "5000000000000000000",
            "_lpAmount": "5000000000000000000",
            "_tokenAmount": "5000000000000000000",
            "_user": "0x1cb30cb181d7854f91c2410bd037e6f42130e860"
          }]
        }
    }
    ```
    
    It has the same data we emitted in our `GraphKitExchange__LiquidityAdded` but we didn't require to have the node setup or to some centralised node provided for this query.
    
    Querying with The Graph Protocol is superfast, decentralized and inexpensive.
    
4. TASK: Use the rest of the functions and query the subgraph for the events they emit.
    

---

# Reference Links

* [Uniswap Book \[V1 Part -1\]](https://jeiwan.net/posts/programming-defi-uniswap-1/)
    
* [Uniswap Book \[V1 Part -2\]](https://jeiwan.net/posts/programming-defi-uniswap-2/)
    
* [Uniswap Book \[V1 Part -3\]](https://jeiwan.net/posts/programming-defi-uniswap-3/)
    
* [Graph Docs](https://thegraph.com/docs/en/developing/creating-a-subgraph/)
    

---

I hope you have learned something today with this quite long blog.

<div data-node-type="callout">
<div data-node-type="callout-emoji">😁</div>
<div data-node-type="callout-text">To learn more about <strong>The Graph Protocol</strong>, join me with <a target="_blank" rel="noopener noreferrer nofollow" href="https://t.me/TheGraph_India" style="pointer-events: none">The Graph India Community</a></div>
</div>

If you have any queries, DM me on [Twitter](https://twitter.com/megabyte0x), [Lenster](https://lenster.xyz/u/megabyte0x) or [LinkedIn](https://linkedin.com/in/megabyte0x).

Happy Learning 🙌🏻

***Keep Building 🧱🚀***