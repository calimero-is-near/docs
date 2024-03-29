---
title: Dashboard
sidebar_position: 2
---

The [Console](https://app.calimero.network/dashboard) dashboard serves as a frontend application for managing Calimero Private Shards. It provides access to all the necessary information about nodes and services. With the dashboard, you can conveniently view all the key details and statistics about your network.

## Key features

1. **Information Access**: The dashboard offers access to important details such as the RPC and Indexer GraphQL URL endpoints. These endpoints enable you to interact with the network and retrieve the data.
2. **Network Statistics**: Get insights into the current state of your network, including the block height, number of transactions, and the total number of accounts.
3. **Scalability**: As your infrastructure requirements expand, the Console allows you to easily scale up the number of validators, RPCs, and indexers in your network. This ensures your network can handle increased traffic and accommodate business growth.


![](../../static/img/dashboard.png)


## Components of the Dashboard

The dashboard consists of several components that provide valuable information about your private shard network:

### Environment

Indicates whether the blockchain environment is Testnet or Mainnet.

### Account

Displays the total number of accounts within each shard.

### Validators

Shows the count of active validator nodes participating in the consensus process, validating transactions, and creating new blocks in the blockchain network.

### Block Height

Represents the number of blocks added to the specific private shard's blockchain.

### Transactions

Provides information about all transactions made within the shard, including FUNCTION_CALL, CREATE_ACCOUNT, ADD_KEY, TRANSFER, and more.

### RPC Node Count

Indicates the number of active RPC nodes that are currently running and capable of responding to client requests for executing procedures or functions on the blockchain network.

### Event Indexer

Displays the count of active indexers that listen to the blockchain network and capture relevant data about specific events generated by smart contracts or other transactions.

### Endpoint

The dashboard offers two endpoints for interacting with the private shard:

- **neard-rpc**: This endpoint is used to interact with the Calimero RPC Service.
- **near-rpc**: This endpoint is used to interact with the Calimero GraphQL Indexer Service.

By utilizing these components and features, you can effectively manage and monitor your Calimero Private Shard network.
