---
title: How to build a Tic Tac Toe game on Calimero
sidebar_position: 1
---

This beginner tutorial will guide you through the process of creating a Tic Tac Toe decentralized application (dApp) using the Calimero network. You'll learn how to set up your environment, build a smart contract, deploy it to the Calimero shard, and interact with the dApp.

<div align="center"><iframe width="560" height="315"  src="https://www.youtube.com/embed/bq7mZ96OJcE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></div>

## Prerequisites

Before you begin, make sure you have the following prerequisites:

- Set up your [Calimero private shard](/docs/getting-started/signup.md).
- Install a code editor like [VSCode](https://code.visualstudio.com/download).
- Install the [NEAR CLI](https://docs.near.org/tools/near-cli#setup) tool.
- Install [Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) on your machine.
- Set up the Rust and WebAssembly (WASM) toolchain. 

## Step 1: Build the Smart Contract

1. Clone the [Calimero examples repository](https://github.com/calimero-is-near/calimero-examples) by running the following command in your terminal:
   
```bash
git clone https://github.com/calimero-is-near/calimero-examples
```

2. Navigate to the `tic-tac-toe` folder and then the `contracts/calimero-tictactoe` directory:

```bash
cd calimero-examples/tic-tac-toe/contracts/calimero-tictactoe
```

3. Compile the Rust smart contract to WebAssembly (WASM) by running the build script:

```bash
./build.sh
```
Once the compilation is complete, you'll find the compiled `.wasm` file of the contract at: `target/wasm32-unknown-unknown/release/tictactoe.wasm`.

## Step 2: Generate an Auth Token

Before deploying the Tic Tac Toe game, you need to generate a Calimero auth token. This token will authenticate and authorize external applications to communicate with your shard.

Follow the steps provided [here](/docs/console/generate_token.md) to generate an auth token for your Calimero shard.

## Step 3: Set up the NEAR CLI

To interact with your Calimero shard using NEAR CLI, you need to set the token value using the `near set-api-key` command.

Follow the steps provided [here](/interact/cli#set-up-the-near-cli-to-access-the-shard-via-cli) to set up the NEAR CLI.


## Step 4: Create a Keypair

A keypair for a shard account consists of a public key and a corresponding private key. To generate a new keypair for the shard account, follow these steps:

1. Set the SHARD_ID environment variable in your command-line interface:

```bash
export SHARD_ID="your_shard_name"
```

Make sure to replace **"your_shard_name"** with the name of your shard.

2. Run the `near generate-key` command to generate a key for your shard:

```bash
near generate-key $SHARD_ID.calimero.testnet --networkId $SHARD_ID-calimero-testnet
```

3. Navigate to the `~/.near-credentials/` folder to locate the generated keypair file in `.json` format. Alternatively, you can use the following command in your terminal:

```bash
cd ~/.near-credentials/network-id/account-id.json
```

:::info
Take note of the **account_id**, **private_key**, and **public_key** values from the **.json** file.
:::

## Step 5: Create a Sub Account

Create a sub account that will be used to deploy the previously built contract. This sub account should be created from the [Custodial account](https://docs.calimero.network/console/custodial#create-custodial-account) in the Calimero Console, and the public key obtained from the generated keypair should be added to the sub account.

Follow the steps [here](https://docs.calimero.network/console/custodial#custodial-account) to set up the sub account and add the public key.

## Step 6: Deploy Your NEAR Contract

To deploy your contract to the private shard, follow these steps:

1. In your cloned repository's directory, open the `deploy_calimero.sh` file. You'll see this command:

```bash
near deploy \
  --accountId "tictactoe.$destination_master_account" \
  --initFunction new --initArgs {} \
  --wasmFile target/wasm32-unknown-unknown/release/tic_tac_toe.wasm \
  --nodeUrl "https://api.calimero.network/api/v1/shards/$shard_id-calimero-testnet/neard-rpc/" \
  --networkId "$shard_id-calimero-testnet"
```

2. Set the SHARD_ID environment variable in your command-line interface:

```bash
export SHARD_ID="your_shard_name"
```

Make sure to replace **"your_shard_name"** with the name of your shard.

3. Run the following command in your terminal to deploy the contract:

```bash
./deploy_calimero.sh $SHARD_ID
```

This command will deploy the application to the NEAR contract using the provided parameters.

You can check the deployed contract on the [Explorer > Transactions](https://app.calimero.network/dashboard/explorer/transactions) page.

![Explorer](https://github.com/calimero-is-near/docs/assets/39309699/e63ce36b-bb48-4d82-8bb5-5da82a25c703/)

## Step 7: Set up Environment Variables

Navigate to the `ui` folder in the repository. Open the `.env.local` file and adjust the environment variables based on your setup:

- **NEXT_PUBLIC_SHARD_ID**: Your Calimero shard ID.
- **NEXT_PUBLIC_WALLET_URL**: Link to testnet myNearWallet
- **NEXT_PUBLIC_CALMERA_URL**: RPC endpoint obtained from the Calimero Console dashboard.
- **NEXT_PUBLIC_CALMERA_TOKEN**: Auth token you generated earlier from the console.
- **NEXT_PUBLIC_CONTRACT_ID**: Your sub account located under the custodial account.

![variables](https://github.com/calimero-is-near/docs/assets/39309699/01573ed1-9661-4c2f-850b-16df5e787578/)

## Step 8: Install Dependencies and Start the Application

1. Open the terminal and navigate to the `ui` folder.
2. Run the following commands to install the necessary dependencies and start the application on `localhost:3000`:
```
yarn install && yarn dev
```
3. Open your browser and go to `localhost:3000` to access the application.

## Step 9: Login and Start a New Game

1. In the application, click on **Login with NEAR**.
2. Select the account associated with the **tic-tac-toe** account.
3. Wait for the login process to complete.

![Login with NEAR](https://github.com/calimero-is-near/docs/assets/39309699/3a1e7584-4e56-45aa-9570-23f568f43c08)

![Login Process](https://github.com/calimero-is-near/docs/assets/39309699/287bba58-da31-4b7d-9dae-f3b1e8620e5f)

**Note**: Two player accounts will be created for this game.

4. Check the transactions to ensure everything synced correctly, including the creation of the accounts on the Calmero shard and the testnet.

![Transaction Check](https://github.com/calimero-is-near/docs/assets/39309699/c8cc9aeb-7cb6-4515-b0a4-9c27e178f202)

## Step 10: Create Second Player

1. Open a different browser and click on **Login with NEAR**.
2. Select the second account associated with the **tic-tac-toe** account.
3. Wait for the login process to complete.

![Login with NEAR](https://github.com/calimero-is-near/docs/assets/39309699/9ce3dfcd-05d5-4857-8705-e26bd08f6081)

4. Check the transactions to ensure everything synced correctly, including the creation of the second player account on the Calmero shard and the testnet.

![Transaction Check](https://github.com/calimero-is-near/docs/assets/39309699/bbb10cb1-375e-4dbb-b642-673785cbd8e3)

## Step 11: Start Playing the Game

1. Go back to the first account and enter the account ID of the player you want to play with (e.g., _simple-tic-tac-toe-2).
2. Click **Start New Game**.

![Start New Game](https://github.com/calimero-is-near/docs/assets/39309699/1660fccf-7d01-4800-9204-4219f59a1656)

3. Approve and close the transaction.

![Transaction Approval](https://github.com/calimero-is-near/docs/assets/39309699/02c3cda4-05fa-4c98-bcf1-fdcc3e2148f7)

4. Similarly, start a new game from the second account and play with the other player (e.g., _simple-tic-tac-toe-1).

![second game](https://github.com/calimero-is-near/docs/assets/39309699/00733601-b7c9-4b24-972f-58eb65b326db)

5. Approve and close the transaction.

## Step 12: View Game Setup

1. You can view the game setup for the first player.

![First Player Game Setup](https://github.com/calimero-is-near/docs/assets/39309699/429b4942-ba30-41a2-a6f3-6f16d1b3cb1f)

2. And the game setup for the second player.

![SecondPlayer Game Setup](https://github.com/calimero-is-near/docs/assets/39309699/0c0fbbce-6efc-4c44-b5bc-f253f3e2b81d)

## Step 12: Continue Playing

1. Click **Continue Game** on both accounts to continue playing.

![Continue Game](https://github.com/calimero-is-near/docs/assets/39309699/0c0fbbce-6efc-4c44-b5bc-f253f3e2b81d)

2. Make your moves on both boards and refresh the pages to see the updated moves.

![Game Moves](https://github.com/calimero-is-near/docs/assets/39309699/64f6b1c7-7005-4954-be8a-f62dd4c9d47e)

3. Continue playing until the game finishes, and the winner will be displayed. You can also check the past games section for a full overview of completed games.

![Game Result](https://github.com/calimero-is-near/docs/assets/39309699/70f9b137-ed00-42c8-89a9-b22b3c254421)

## Conclusion

Congratulations! You have successfully created a simple tic-tac-toe game with two players on the blockchain using the Calmero private shard. This example demonstrates how smart contracts and blockchain can be implemented into a decentralized application or game.