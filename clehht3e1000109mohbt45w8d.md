---
title: "Building Cross-Chain NFT using Router Protocol's CrossTalk Library"
seoTitle: "Building Cross-Chain NFT using Solidity for Polygon and Avalanche."
seoDescription: "Solidity Tutorial for building a Cross-Chain NFT (ERC721), to transfer an NFT from Avalanche to Polygon using Router Protocol's Cross-Talk Library."
datePublished: Thu Feb 23 2023 19:23:54 GMT+0000 (Coordinated Universal Time)
cuid: clehht3e1000109mohbt45w8d
slug: router-portocol-crosstalk
canonical: https://blog.developerdao.com/router-portocol-crosstalk
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677178523836/98083569-d0e9-458c-93a6-133a2a0056d6.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1677179834743/881ff369-b31c-4082-bd61-ff1ebdcfc0ca.png
tags: solidity, nft, polygon, avalanche, hardhat

---

**gm gm gm!!!**

Today we will write an NFT (ERC721) smart contract, allowing sending and receiving an NFT from different chains. To transfer our NFTs between chains, we will use [**Router Cross-Talk Library**](http://devnet-docs.routerprotocol.com/crosstalk).

We will transfer an NFT from Avalanche Fuji Testnet to Polygon Mumbai Testnet.

---

If you are more of a video tutorial fan, then this is for you 👇🏻

%[https://youtu.be/NUb7Nt8lges] 

---

## **Understanding the Fundamentals**

Before we start, let's look at the basics, so we're all on the same page.

### What is Chain-Interoperability?

Chain interoperability is the capability of different blockchain networks or systems to interact and exchange information and resources. In simpler terms, blockchain systems can work together as a unified network instead of operating in isolation.

For example, we could use one blockchain to store financial transactions and another blockchain to store information about a supply chain. If these two blockchains are interoperable, they can create a new application combining both benefits.

Chain interoperability is a complex aspect of blockchain technology, requiring a deep understanding of cryptography, consensus algorithms, and network protocols.

Implementing interoperability protocols like the Router Protocol and Cosmos Network provides a common language and infrastructure for transferring information and assets between networks, creating a more interoperable and connected blockchain ecosystem.

The continued advancement of interoperability standards and protocols will play a vital role in shaping the future of blockchain technology and its impact on various industries and society as a whole.

### **What is the CrossTalk Library?**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676274749092/4dd191a2-8ac9-441f-8823-c4accff0cb7d.png?auto=compress,format&format=webp align="left")

Router's CrossTalk library is an extensible cross-chain framework that enables seamless state transitions across multiple chains. In simple terms, this library leverages Router's infrastructure to allow contracts on one chain to communicate with contracts deployed on some other chain.

It consists of 3 essential functions, which help build a cross-chain application together.

1. `requestToDest` sends cross-chain requests with a message from the source contract to the gateway contract on the source chain. The message will then be forwarded to the Gateway Contract on the destination chain.
    
2. `handleRequestFromSource` is to send the request (with some message) from the source contract (on the source chain) to the destination contract (on the destination chain), where it executes all the functions defined in it and then will return the data(if any).
    
3. `handleCrossTalkAck` is to receive the acknowledgment on the source chain from the gateway contract on the destination chain, which is in the boolean value stating whether the functions on the destination chain got executed.
    

> *Learn more about Cross Talk Library from* [*here*](http://devnet-docs.routerprotocol.com/crosstalk)*.*

The following figure shows how we will implement CrossTalk Librabry in our contract.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676283371459/897b1335-3c1d-4443-8768-501cf51dd835.jpeg?auto=compress,format&format=webp align="left")

## **Implementing the Contract 👨🏻‍💻**

Now that we have learned the fundamentals let's start developing our contract.

### Setting up the Environment

Before we can start programming, we need to set up our development environment.

#### Creating a Project

Create a project folder and initialize NPM.

```bash
$ mkdir crossERC721 && cd crossERC721
$ npm init -y
```

#### **Installing Dependencies**

Install dependencies and initialize a Hardhat project.

> ***Drink some water while all the dependencies to install!***

```bash
$ npm install --save-dev hardhat ts-node typescript chai @types/node @types/mocha @types/chai dotenv
$ npx hardhat
```

Select "TypeScript Project" and then press "Enter" three times. The result should look like the following figure; it will start installing dependencies.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676123939215/0062cb6b-546e-410a-a90e-8d2a81f61739.png?auto=compress,format&format=webp align="left")

#### Configuring Hardhat

Open the folder with an IDE (I am using VSCode) to configure Hardhat.

File `hardhat.config.ts`:

```bash
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config({ path: ".env" });
require("@nomiclabs/hardhat-waffle");

const PRIVATE_KEY = process.env.PRIVATE_KEY;
const ALCHEMY_POLYGON_URL = process.env.ALCHEMY_POLYGON_URL;
const POLYGON_SCAN_KEY = process.env.POLYGON_SCAN_KEY;
const AVALANCHE_URL = process.env.AVALANCHE_URL;
const AVALANCHE_SNOWTRACE_KEY = process.env.AVALANCHE_SNOWTRACE_KEY;

module.exports = {
  solidity: "0.8.18",
  networks: {
    mumbai: {
      url: ALCHEMY_POLYGON_URL,
      accounts: [PRIVATE_KEY],
    },
    fuji: {
      url: AVALANCHE_URL,
      accounts: [PRIVATE_KEY],
      chainId: 43113,
    },
  },
  etherscan: {
    apiKey: {
      polygonMumbai: POLYGON_SCAN_KEY,
      avalancheFujiTestnet: AVALANCHE_SNOWTRACE_KEY,
    },
  },
};
```

#### **Setting up the Environment Variables**

To set up the env variables in development, add them into the `.env` file.

File `.env`:

```bash
ALCHEMY_POLYGON_URL= "abcabc"
PRIVATE_KEY=abcbcabc
POLYGON_SCAN_KEY= abcabcabc
AVALANCHE_SNOWTRACE_KEY = abcabcabc
AVALANCHE_URL= "https://api.avax-test.network/ext/bc/C/rpc"
```

The missing pieces and where to find them:

* `ALCHEMY_POLYGON_URL` - [**dashboard.alchemy.com**](http://dashboard.alchemy.com)
    
* `PRIVATE_KEY` - your development account in MetaMask.
    
* `POLYGON_SCAN_KEY` - [**polygonscan.com/myapikey**](http://polygonscan.com/myapikey)
    
* `AVALANCHE_SNOWTRACE_KEY` -[**snowtrace.io/myapikey**](http://snowtrace.io/myapikey)
    

> ***Congrats 🤩🎉 Your set up your environment for development.***

#### Installing the Contract Dependencies

Install OpenZeppelin contracts and dependencies for cross-chain functionalities:

```bash
$ npm install @openzeppelin/contracts evm-gateway-contract @routerprotocol/router-crosstalk-utils
```

Finallyyy... done with DEPENDENCIESsss...

### Writing the Contract Code 💪🏻

Create a new Solidity file at `contracts/CrossERC721.sol`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676125375803/2023c2dc-8592-4fb6-9290-29d1de68a7c1.png?auto=compress,format&format=webp align="left")

#### Implementing the Contract Definition

First, we need to set up our contract definition. The contract will use different dependencies to facilitate the cross chain interaction.

File `CrossERC721.sol`:

```solidity
// SPDX-License-Identifier: Unlicensed 
pragma solidity 0.8.18;

import "evm-gateway-contract/contracts/ICrossTalkApplication.sol"; import "evm-gateway-contract/contracts/Utils.sol"; import "@routerprotocol/router-crosstalk-utils/contracts/CrossTalkUtils.sol"; import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract CrossERC721 is ERC721, ICrossTalkApplication {}
```

#### Declaring the State Variables

File `CrossERC721.sol`:

```solidity
// ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // Address of the Owner of the contract.
    address public admin;

    // Address of the gateway contract on the chain will contract deployed. 
    address public gatewayContract; 

    // Gas limit required to handle cross-chain request on the destination chain
    uint64 public destGasLimit;

    // chain type + chain id => address of our contract in bytes
    mapping(uint64 => mapping(string => bytes)) public ourContractOnChains;

    // Transfer parameter which include tokenId and the address(in bytes) of the receiver on destination chain.
    struct TransferParams {
        uint256 nftId;
        bytes recipient;
    }
}
```

#### **Implementing the Constructor**

When we implement the constructor, we set the state variables in the constructor and mint an NFT (ERC721) to the `msg.sender`.

File `CrossERC721.sol`:

```solidity
// ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // ...

    /// @notice Constructor to initialize the contract.
    /// @param gatewayAddress - Address of the gateway contract on the chain on which we will deploy the contract
    /// @param _destGasLimit - Gas limit required to handle cross-chain request on the destination chain.
    /// @param tokenId - Token Id of the NFT to be minted for testing.
    constructor(
        address payable gatewayAddress,
        uint64 _destGasLimit,
        uint256 tokenId
    ) ERC721("CrossERC721", "cerc721") {
        gatewayContract = gatewayAddress;
        destGasLimit = _destGasLimit;
        admin = msg.sender;
        _mint(msg.sender, tokenId);
    }
}
```

#### **Implementing the setContractOnChain Method**

Implement `setContractOnChain` to map all the contract addresses of the contract on different chains. The mapping is used to verify the contract with which we will interact.

File `CrossERC721.sol`:

```solidity
// ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // ...

    /// @notice Constructor to initialize the contract.
    /// @param gatewayAddress - Address of the gateway contract on the chain on which we will deploy the contract
    /// @param _destGasLimit - Gas limit required to handle cross-chain request on the destination chain.
    /// @param tokenId - Token Id of the NFT to be minted for testing.
    constructor(
        address payable gatewayAddress,
        uint64 _destGasLimit,
        uint256 tokenId
    ) ERC721("CrossERC721", "cerc721") {
        gatewayContract = gatewayAddress;
        destGasLimit = _destGasLimit;
        admin = msg.sender;
        _mint(msg.sender, tokenId);
    }
}
```

#### **Implementing the setContractOnChain Method**

Implement `setContractOnChain` to map all the contract addresses of the contract on different chains. The mapping is used to verify the contract with which we will interact.

File `CrossERC721.sol`:

```solidity
// ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // ...

    /// @notice Function to map all the contract addresses on different chains.
    /// @param chainType - Type of the chain specified by the Router Protocol on which we will deploy the contract
    /// @param chainId - Chain Id of the chain on which the contract is deployed.
    /// @param contractAddress - Address of the contract on the chain
    function setContractOnChain(
        uint64 chainType,
        string memory chainId,
        address contractAddress
    ) external {
        require(msg.sender == admin, "only admin");

        // CrossTalkUtils.toBytes() is a function which converts the address to bytes.
        ourContractOnChains[chainType][chainId] = CrossTalkUtils.toBytes(
            contractAddress
        );
    }
}
```

#### **Implementing the transferCrossChain Method**

Implement `transferCrossChain`, the core function of our contract. It burns the NFT owned by `msg.sender` on the source chain and then sends the request of transferring using `CrossTalkUtils.singleRequestWithoutAcknowledgement`, which sends the request to the gateway contract on the destination chain.

File `CrossERC721.sol`:

```solidity
        // ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // ...

    /// @notice Function to transfer the NFT from the source chain to the destination chain.
    /// @param chainType - Type of the chain specified by the Router Protocol on which the nft needs to transferred.
    /// @param chainId - Chain Id of the destination chain.
    /// @param expiryDurationInSeconds - Expiry duration in seconds of the request.
    /// @param destGasPrice - Gas price required to handle the cross-chain request on the destination chain.
    /// @param _nftId - Token Id of the NFT to be transferred.
    /// @param _recepient - Address of the recipient on the destination chain.
    function transferCrossChain(
        uint64 chainType,
        string memory chainId,
        uint64 expiryDurationInSeconds,
        uint64 destGasPrice,
        uint256 _nftId,
        address _recepient
     ) public payable {
         require(
             keccak256(ourContractOnChains[chainType][chainId]) !=
                 keccak256(CrossTalkUtils.toBytes(address(0))),
             "ERR:CROSS_CHAIN_CONTRACT_NOT_SET"
         );

         TransferParams memory transferParams = TransferParams(
             _nftId,
             CrossTalkUtils.toBytes(_recepient)
         );

         require(_ownerOf(transferParams.nftId) == msg.sender, "ERR:NOT_OWNER");

         // Burn the NFT of the user on the source chain.
         _burn(transferParams.nftId);

         // Encode the transfer parameters to bytes for sending it as payload to the gateway contract.
         bytes memory payload = abi.encode(transferParams);

         uint64 expiryTimestamp = uint64(block.timestamp) +
             expiryDurationInSeconds;

         Utils.DestinationChainParams memory destChainParams = Utils
             .DestinationChainParams(
                 destGasLimit,
                 destGasPrice,
                 chainType,
                 chainId
         );

         // Call the singleRequestWithoutAcknowledgement() function 
         // to transfer the NFT from the source chain to the
         // destination chain without any acknowledgment.
         CrossTalkUtils.singleRequestWithoutAcknowledgement(
             gatewayContract,
             expiryTimestamp,
             destChainParams,
             ourContractOnChains[chainType][chainId],
             payload
         );
    }
}
```

#### **Implementing the handleRequestFromSource Method**

Implement `handleRequestFromSource` for the contract on the destination chain. Since we must deploy the contract on every chain we interact with, we must include the receiving function in the same contract. We mint the NFT for the receiver's address on the destination chain in this function.

File `CrossERC721.sol`:

```solidity
 // ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // ...

    /// @notice Function to handle the request from the gateway contract on the destination chain. It manages data received and calls the function(s).
    /// @param srcContractAddress is the contract address on the source chain.
    /// @param payload is the data received from the source chain in bytes.
    /// @param srcChainId is the chain id of the source chain.
    /// @param srcChainType is the chain type of the source contrac specified by the Router Protocol.
    function handleRequestFromSource(
        bytes memory srcContractAddress,
        bytes memory payload,
        string memory srcChainId,
        uint64 srcChainType
    ) external override returns (bytes memory) {
        require(msg.sender == gatewayContract, "ERR:NOT_GATEWAY_CONTRACT");
        require(
            keccak256(srcContractAddress) ==
                keccak256(ourContractOnChains[srcChainType][srcChainId]),
            "ERR:CONTRACT_NOT_FOUND"
        );

        TransferParams memory transferParams = abi.decode(
            payload,
            (TransferParams)
        );

        // Mint the NFT for the recipient address on the destination chain.
        _mint(
            CrossTalkUtils.toAddress(transferParams.recipient),
            transferParams.nftId
        );

        // Since we don't want to return any data, we will just return empty string
        return "";
    }
}
```

#### **Implementing the handleCrossTalkAck Method**

Implement `handleCrossTalkAck`, which handles the acknowledgment by the gateway contract after the transaction on the destination chain executes. Since we are using `singleRequestWithoutAcknowledgement` type of request, we will leave this function empty.

File `CrossERC721.sol`:

```solidity
// ...

contract CrossERC721 is ERC721, ICrossTalkApplication {
    // ...

    /// @notice Function to handle the acknowledgement received by the gateway contract for the functions executed on the destination chain.
    /// Since we are not expecting any acknowledgement, we will just keep this function empty.
    /// @param eventIdentifier is the event identifier of the request.
    /// @param execFlags is the array of boolean values which specifies whether the function executed successfully or not on destination chain.
    /// @param execData is the array of bytes which contains the data returned by the function executed on the destination chain.
    function handleCrossTalkAck(
        uint64 eventIdentifier,
        bool[] memory execFlags,
        bytes[] memory execData
    ) external view override {}
}
```

**CONGRATSSS, you wrote the cross-chain contract 🎉**

> *You can find the complete contract* [*on GitHub*](https://github.com/megabyte0x/CrossERC721/blob/main/contracts/CrossERC721.sol)*.*

## **Deploying the Contract**

We will deploy our contract on **Polygon Mumbai Testnet** and **Avalanche Fuji Testnet**.

For *BONUS!!!* we will verify it on their respective block explorers, [**PolygonScan**](https://mumbai.polygonscan.com/) and [**Snowtrace**](https://testnet.snowtrace.io/)

### Creating a Deployment Script

Create a file at `scripts/deploy.ts` with the following content.

File `deploy.ts`:

```solidity
import { ethers } from "hardhat";

async function main() {
  const network = await hre.network;

  const gatewayContract =
    network.config.chainId == 43113
      ? "0x517f256cc48145c25c27cf453f6f5006e5266543"
      : "0x8EA05371Eb360Eb79c295375CB2cCE9191EFdaD0";

  const tokenId = network.config.chainId == 43113 ? 1 : 2;

  const CrossERC721 = await ethers.getContractFactory("CrossERC721");
  const crossERC721 = await CrossERC721.deploy(
    gatewayContract,
    1000000,
    tokenId
  );

  await crossERC721.deployed();

  console.log("CrossERC721 deployed to:", crossERC721.address);

  console.log("Sleeping.....");
  await sleep(40000);

  await hre.run("verify:verify", {
    address: crossERC721.address,
    constructorArguments: [gatewayContract, 1000000, tokenId],
  });
}
function sleep(ms: number) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exitCode = 1;
  });
```

### **Executing the Deployment Script**

Run the following commands in the terminal in the root directory:

```bash
$ npx hardhat run scripts/deploy.ts --network mumbai
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676272427278/688b7c8b-441a-4f19-ae1e-33fc475d52de.png?auto=compress,format&format=webp align="left")

> SAVE the above👆 address and URL
> 
> 0xB905345D930707C992ec768Cf748AaBc0D3207Da
> 
> [**https://mumbai.polygonscan.com/address/0xB905345D930707C992ec768Cf748AaBc0D3207Da#code**](https://mumbai.polygonscan.com/address/0xB905345D930707C992ec768Cf748AaBc0D3207Da#code)

```bash
  $ npx hardhat run scripts/deploy.ts --network fuji
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676272433212/3946d79e-d8f4-4f0f-91f2-94566389b47f.png?auto=compress,format&format=webp align="left")

> SAVE the above👆 URL
> 
> 0xC0438dF8A5008Af185B36F4f2C38be410C9ce95d
> 
> [**https://testnet.snowtrace.io/address/0xC0438dF8A5008Af185B36F4f2C38be410C9ce95d#code**](https://testnet.snowtrace.io/address/0xC0438dF8A5008Af185B36F4f2C38be410C9ce95d#code)

You deployed and verified your contract on both chains. **Awesome!!! 🎉🎉**

## **Interacting with the Contract**

To interact with our freshly deployed contracts, open the two URLs you saved in different tabs in your browser.

### On the MumbaiScan tab

1. click "Write Contract".
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676216219347/5b117804-c4ce-4da1-b85d-f172ea23a1d7.png?auto=compress,format&format=webp align="left")
    
2. Click on "Connect to Web3" and connect your MetaMmask wallet with it.
    
3. Scroll down to "setContractOnChain".
    
4. Enter the values. Here the `contractAddress` value is the address on the Fuji Network.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676272805613/114c36ea-dcd7-4080-a2ea-71d15fa72c27.png?auto=compress,format&format=webp align="left")
    
5. Click "Write" and then "Confirm" the transaction.
    

### On the SnowTrace tab

1. Click "Write Contract".
    
2. Click "Connect to Web3".
    
3. Scroll down to `setContractOnChain`
    
4. Enter the values.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676272857949/8b6d5ee2-5186-4022-a48f-087625514ff6.png?auto=compress,format&format=webp align="left")
    
5. Click "Write" and "Confirm" the transaction.
    
    > We have set the contracts on both the source and destination chains.
    
6. Scroll to the `transferCrossChain`
    
7. Enter the values.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676273060103/040ee804-4354-42ed-8f13-387dfdb5510a.png?auto=compress,format&format=webp align="left")
    
8. Click "Write" and "Confirm" the transaction.
    
9. Copy `recepient` and paste it into the destination chain (POLYGON MUMBAI) block explorer.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676273200301/8fa1ee68-3197-4059-abb4-c49f8d301d85.png?auto=compress,format&format=webp align="left")
    

**Hoorrayyy!!! 🎉🎉**

You just transferred an NFT from Avalanche Fuji Testnet to Polygon Mumbai Testnet. You can do the same with NFTs on Polygon Mumbai Testnet.

**PS: You can check out the whole project here👇🏻**

%[https://github.com/megabyte0x/CrossERC721] 

So, we have created an ERC721 with the implementation of Router Protocol's Cross Talk Library which helped us to transfer our NFT from Avalanche to Polygon.

You can do the same with ERC20 and others, and make your application more **SCALABLE, SECURE, and DENCTRALISED**.

---

Connect with me on Lens🌿\[@[@megabyte0x](@megabyte0x)**.lens\] or Twitter\[**@[@megabyte0x](@megabyte0x)**\].**

Also, feel free to share your learnings and reach out to me if you've any doubts or questions for me.

Happy building! 🛠️

**WAGMI🚀**