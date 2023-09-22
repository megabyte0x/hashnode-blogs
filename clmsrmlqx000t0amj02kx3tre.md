---
title: "Deploy your OWN Hyperlane"
seoTitle: "How to deploy Hyperlane?"
seoDescription: "Guide to deploy Hyperlane on Polygon zkEVM, Mumbai Testnet and Sepolia."
datePublished: Thu Sep 21 2023 06:01:54 GMT+0000 (Coordinated Universal Time)
cuid: clmsrmlqx000t0amj02kx3tre
slug: deploy-your-own-hyperlane
canonical: https://blog.hyperlaneindia.xyz/permissionless-deployment
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1694936945010/8872b468-d139-4873-87e0-6c4aa3f0af1b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1694936959873/18179db9-f8ef-44b7-a926-6351ecfbead5.png
tags: typescript, blockchain, ethereum, solidity, crosschain-bridge

---

gm gm gm!!!  
***Hyperlane is a Permissionless Protocol*** which means that anyone can deploy their own Hyperlane Protocol on any chain without requiring any permission from the core team.

In this tutorial, we will learn how to deploy Hyperlane on [Polygon zkEVM](https://wiki.polygon.technology/docs/zkevm/) Testnet, [Polygon](https://wiki.polygon.technology/docs/pos/getting-started/) Mumbai Testnet and Sepolia.

There are 5 steps we need to follow so LFG🚀

# Prerequisites

* [Yarn](https://classic.yarnpkg.com/lang/en/docs/cli/install/)
    
* [Git and GitHub Knowledge](https://github.com/)
    
* Understanding of Blockchain
    
* Solidity Experience (BONUS)
    

> Before moving forward, if you like to understand Hyperlane in-depth (which you should!) check this out:
> 
> %[https://blog.megabyte0x.xyz/a-z-of-hyperlane] 

# Setup Keys

When you deploy Hyperlane Protocol it includes Smart Contracts, Validators and Relayers therefore we will be requiring **Key Pair** for each of them.

## Keys for Deployment

To deploy smart contracts you will need to have a **key pair** with the funds for all the chains you will be deploying.

To create the **key pair**, follow the steps:

* Open your Terminal and enter the following command
    
    ```bash
    cast wallet new
    ```
    
    This will generate a new **key pair** for you, which will look like this 👇🏻
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695271307360/af69b0d0-75a9-421b-b7c2-53147bc13899.png align="center")
    
    <div data-node-type="callout">
    <div data-node-type="callout-emoji">⚠</div>
    <div data-node-type="callout-text">THIS IS A TESTING WALLET.</div>
    </div>
    
* Save your Public Address and Private Key.
    
* Fund your Address with the tokens of all the networks.
    
    * For ETH on Polygon zkEVM Testnet and Polygon Mumbai Testnet use [Polygon Faucet.](https://faucet.polygon.technology/)
        
    * For ETH on Sepolia use [Alchemy Faucet](https://sepoliafaucet.com/).
        

## Keys for Validators

For this tutorial, we will be using the default [Hyperlane's Multisig ISM](https://docs.hyperlane.xyz/docs/protocol/sovereign-consensus/multisig-ism). Multisig ISM is a module that takes several signatures from validators before completing the transaction. Therefore, we will be using **4 addresses** with threshold **one**.

You can take any 4 of your Public Addresses, and keep them handy for later use.

> Make sure to have a Private Key secured for the Public addresses you will be using.  
> You can generate Key Pairs using the above method.

# Setting up the Repo

The Hyperlane Core team provided us with a repository that you can use to deploy Hyperlane on your preferred chain.

Follow these steps to set up the Hyperlane Deployment repo:

* Open your terminal at your preferred directory.
    
* ```bash
      git clone https://github.com/hyperlane-xyz/hyperlane-deploy.git
    ```
    
* ```bash
      cd hyperlane-deploy
    ```
    
* ```bash
      yarn install
    ```
    

# Adding the custom chain

Hyperlane's core team has provided a few chains in the SDK. You can check them out [here](https://docs.hyperlane.xyz/docs/resources/addresses).

To add a chain apart from the provided chains, follow these steps:

* Open `config/chains.ts` There you will find the configuration example for adding new chains.
    
* Add the following configuration for the Polygon zkEVM Testnet:
    
    ```typescript
      polygonzkevmtestnet: {
        name: 'polygonzkevmtestnet',
        chainId: 1442,
        protocol: ProtocolType.Ethereum,
        nativeToken: {
          name: 'ether',
          symbol: 'ETH',
          decimals: 18,
        },
        rpcUrls: [
          {
            http: 'https://rpc.public.zkevm-test.net',
          },
        ],
        blockExplorers: [
          {
            name: 'Polygon Scan',
            url: 'https://testnet-zkevm.polygonscan.com',
            apiUrl: 'https://api-zkevm.polygonscan.com/api',
          },
        ],
        isTestnet: true,
      },
    ```
    

# Adding the Validator Keys

For this tutorial, we are using the default [Multisig](https://docs.hyperlane.xyz/docs/protocol/sovereign-consensus/multisig-ism) as our Interchain Security Module.

Follow these steps, for setting up the keys:

* Open `config/multisig_ism.ts`. There you will find the configuration example for adding new chains and their keys.
    
* Add the keys for your validator:
    
    ```typescript
      polygonzkevmtestnet: {
        threshold: 1,
        type: ModuleType.LEGACY_MULTISIG,
        validators: [
          '0xabcabcabcabcabcabcabcabcabcabcabcabcabca',
          '0xabcabcabcabcabcabcabcabcabcabcabcabcabca',
          '0xabcabcabcabcabcabcabcabcabcabcabcabcabca',
          '0xabcabcabcabcabcabcabcabcabcabcabcabcabca',
        ],
      },
    ```
    
* `threshold` is 1 which means only one of the validator's signatures is required.
    

> Woohoooo!!! You are done with the setup for the deployment of HYPERLANE on Polygon zkEVM testnet with remotes Mumbai and Sepolia.

---

# Deploying the Hyperlane

To deploy Hyperlane, run the following command on your terminal.

```bash
DEBUG=hyperlane* yarn ts-node scripts/deploy-hyperlane.ts --local polygonzkevmtestnet \
  --remotes mumbai sepolia \
  --key 0xbeda2b3910bae7dbfb4e65e9c0931f001066d4f0df257b77ad55093fa271bde2
```

In the above command,

* we are running the `deploy-hyperlane.ts` script
    
* with `polygonzkevmtestnet` as `local` chain and
    
* `mumbai` and `sepolia` as remotes. Since `mumbai` and `sepolia` are already provided by the Hyperlane SDK we don't need to add the configurations again.
    
* the value after `key` param is your **private key** to the address that contains funds for all the networks.
    

After running the command, wait for a few minutes till your terminal looks like this 👇🏻

---

Congrats!!! You just **Deployed** Hyperlane on Polygon zkEVM Testnet.

But to test this out, you will need to run your Validators and Relayers for all the chains you deployed (local + remote). To learn how to setup Validators and Relayers check them out 👇🏻

%[https://blog.hyperlaneindia.xyz/hyperlane-relayers-setup-guide] 

%[https://blog.hyperlaneindia.xyz/hyperlane-validators-setup-guide] 

---

Thanks for going through the whole blog, I hope you learned something today.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">If you wanna explore more check out the <a target="_blank" rel="noopener noreferrer nofollow" href="https://docs.hyperlane.xyz" style="pointer-events: none">Hyperlane Docs</a>, and if you have any queries get them resolved in <a target="_blank" rel="noopener noreferrer nofollow" href="http://t.me/c/1264208834/1" style="pointer-events: none">Hyperlane India Telegram Channel.</a></div>
</div>

Let me know if you have any doubts on [*Twitter*](https://twitter.com/megabyte0x)*,* [*Lenster*](https://lenster.xyz/u/megabyte0x) *or* [*LinkedIn*](https://linkedin.com/in/megabyte0x)*.*

***Happy Learning 🙌🏻***

***Keep Building 🧱🚀***