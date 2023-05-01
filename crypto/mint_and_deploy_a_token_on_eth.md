Creating a token on the Ethereum network typically involves creating a smart contract that follows a specific standard, such as the ERC-20 or ERC-721 token standards. In this example, I'll walk you through creating a simple ERC-20 token using the Solidity programming language.

1. Install necessary tools:

You'll need to install Node.js, npm (comes with Node.js), and Truffle. To install Truffle globally, open your terminal and run:

```bash
npm install -g truffle
```

2. Create a new Truffle project:

```bash
mkdir MyToken
cd MyToken
truffle init
```

This will create a new directory called `MyToken` and initialize a Truffle project inside it.

3. Create a new Solidity contract file:

In the `contracts` directory, create a new file called `MyToken.sol` with the following content:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("MyToken", "MTK") {
        _mint(msg.sender, initialSupply);
    }
}
```

This code imports the OpenZeppelin ERC20 smart contract library and creates a new contract called `MyToken` that extends the ERC20 contract. The constructor takes an initial supply as an argument and mints that amount of tokens to the contract deployer's address.

4. Install OpenZeppelin Contracts:

OpenZeppelin Contracts is a library for secure smart contract development. Run the following command in your terminal to install it:

```bash
npm install @openzeppelin/contracts
```

5. Configure the Truffle migration script:

In the `migrations` directory, create a new file called `2_deploy_token.js` with the following content:

```javascript
const MyToken = artifacts.require("MyToken");

module.exports = function (deployer) {
  const initialSupply = web3.utils.toWei("1000000", "ether");
  deployer.deploy(MyToken, initialSupply);
};
```

This script will deploy the `MyToken` contract with an initial supply of 1,000,000 tokens.

6. Deploy the token:

First, you need to set up a local Ethereum blockchain for development purposes. You can use Ganache for this:

```bash
npm install -g ganache-cli
ganache-cli
```

In a new terminal window, navigate to your project directory (`MyToken`) and run the following command to deploy the token on the local blockchain:

```bash
truffle migrate
```

7. Interact with the token:

You can use Truffle console to interact with the deployed token:

```bash
truffle console
```

In the console, you can run commands to interact with the token. For example:

```javascript
const token = await MyToken.deployed()
const accounts = await web3.eth.getAccounts()
const balance = await token.balanceOf(accounts[0])
console.log(balance.toString())
```

These commands will retrieve the balance of the first account (which should hold the initial supply of tokens).

That's it! You've created a simple ERC-20 token on the Ethereum network using Solidity and Truffle. You can customize the token further, add more features, or deploy it on the Ethereum mainnet or a testnet using a service like Infura.

## Deploy on Ethereum mainnet

To deploy your MyToken contract on the Ethereum mainnet and list it on Uniswap, follow the steps below:

1. Get an Ethereum wallet and mainnet ETH:

First, you'll need an Ethereum wallet to hold your mainnet ETH. You can create a wallet using a tool like MetaMask. Once you have your wallet set up, you'll need to obtain some mainnet ETH to pay for gas fees associated with deploying your contract and interacting with Uniswap.

2. Configure Truffle for Ethereum mainnet:

To deploy the MyToken contract on the Ethereum mainnet, you need to configure Truffle to use the mainnet. In your `truffle-config.js` file, add the following code:

```javascript
const HDWalletProvider = require('@truffle/hdwallet-provider');
const mnemonic = 'your wallet mnemonic or seed phrase here';

module.exports = {
  networks: {
    mainnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://mainnet.infura.io/v3/YOUR-PROJECT-ID'),
      network_id: 1,
      gas: 5500000,
      confirmations: 2,
      timeoutBlocks: 200,
      skipDryRun: true
    },
    // ...
  },
  // ...
};
```

Make sure to replace `'your wallet mnemonic or seed phrase here'` with your wallet's mnemonic or seed phrase, and replace `YOUR-PROJECT-ID` with your Infura project ID.

You also need to install the `@truffle/hdwallet-provider` package:

```bash
npm install @truffle/hdwallet-provider
```

3. Deploy MyToken to the Ethereum mainnet:

Run the following command to deploy your MyToken contract on the Ethereum mainnet:

```bash
truffle migrate --network mainnet
```

Take note of the deployed contract address for future use.

4. Add liquidity to Uniswap:

To list your token on Uniswap and create a trading pair, you'll need to add liquidity to a Uniswap pool. First, visit the Uniswap app at https://app.uniswap.org/. Connect your wallet by clicking "Connect to a wallet" in the top right corner.

Next, click on "Pool" in the top menu, then click "Create a pool." Now, click "Select a token" and paste your MyToken contract address. Uniswap should recognize your token. Select a trading pair for your token, such as ETH or USDC.

Set the initial price and the amount of tokens you want to provide as liquidity. Make sure you have enough of the paired asset (ETH or USDC) in your wallet to cover the initial liquidity.

Click "Approve MyToken" and confirm the transaction in your wallet. Once the transaction is confirmed, click "Supply" and confirm the transaction to create the liquidity pool and list your token on Uniswap.

Now, your MyToken should be available for trading on Uniswap.

Note: Be cautious when providing your mnemonic or seed phrase, as this grants access to your wallet. It's recommended to use a separate wallet for deployment and not your main wallet holding your funds. Additionally, deploying to the mainnet and adding liquidity on Uniswap can be expensive due to gas fees. Make sure you have enough ETH to cover these costs.