---
title: Discover the Mempool and retrieve transactions
id: mempool
---

**By Anita Diamond**

# Discovering the Mempool and retrieving transactions


## Introduction

The mempools (memory pools) or transaction pools are the waiting areas for pending transactions on blockchain networks prior to them being included in a block. Every node has its own unique mempool, so there are as many mempools as there are nodes. In addition, each node can be configured differently to others so there is no single view of ‘the mempool’.

All chains have mempools. Ethereum, Polygon and Binance Smart Chain mempools work in a similar way. 

In this tutorial, you will:

* Learn what the mempool is and how it can be used for profit as well as dark uses of the mempool.
* Learn how to query the mempool and retrieve pending transactions on the Ethereum network using Geth. 


## What is a mempool?

When you perform a web3 transaction, you pass your transaction to the network and wait for it be confirmed and completed. The node that receives your transaction validates it and it is then passed to the mempool. 

This makes working with mempool data both resource-intensive and error-prone for most builders and traders.

Transactions exist in the mempool as pre-chain data where value is in motion. They reside here whilst they are ordered, prioritized according to fees and as blocks are constructed. On-chain data represents value-at-rest or static, confirmed transactions.  

However, sometimes your transaction gets stuck as ‘pending’ beyond a reasonable time. In these circumstances, it is useful to know what has happened. 

While on-chain transactions are immutable, in-flight transactions can be overwritten with a replacement transaction. This is the case when ‘cancel’ and ‘speed up’ transactions are executed.

There are also certain conditions that can result in all transactions being pushed from a mempool. In this scenario, the transaction is not added to the block. 

The mempool is often referred to as the dark forest as The mempool can be used as a source of profit as well as utility. 


## Uses of the Mempool

Because mempool data is by definition pre-chain, there is no single source of mempool-truth. Each single node contains a partial view of global mempool data. There are no easy ways to work with mempool data although this is an emerging area. Check out [https://www.blocknative.com](https://www.blocknative.com)


### Estimating Gas Fees:

Gas Fees depend on two key factors: The complexity of a transaction and the current congestion on the network (the demand for space in a block). When setting a gas fee for a transaction, it pays to consider the time sensitivity of the transaction and the gas price of other transactions currently in the mempool. It is also useful to know what the spread of gas fees is. You can then use this information to inform your setting of a gas fee. 


### Arbitrage: 

An arbitrage transaction balances prices among several markets. For example, if you buy 2 of a token on Uniswap, that token price will now be higher on Uniswap (supply vs demand). However, the price will not have changed on SushiSwap, so we could add another trade to buy 1 of that token on SushiSwap at a cheaper price and sell it on Uniswap at a higher price. This arbitrage transaction balances the prices between Uniswap and SushiSwap markets.


### Yield farming:

Observe transaction movements between DeFi applications to detect yield farming profitability changes.


### Liquidity providing: 

Detect upcoming liquidity movements in and out of DeFi applications and adapt a trading strategy accordingly.


## Dark Uses of the Mempool

The mempool is a fertile target for actors and bots looking to extract value from your transaction in a number of ways. ‘Maximum extractable value ’(MEV) refers to the value in terms of profit that can be gained by front-running transactions and manipulating the process order of transactions on the blockchain. 


### Front-runners

Front runners monitor the mempool and use this information to perform arbitrage trades to maximize their own profits at your expense. Here is a front running example:  I see in the mempool a transaction TX1 buying 1000 ‘Made Up Tokens’ and spending 0.02 Gwei in gas fees. I can now create TX2 buying 10 ‘Made Up Tokens’ and spend 0.1 Gwei in gas fees. In spending more gas, my transaction will precede the first transaction. When TX1 has been completed and pushed the price of ‘Made Up Tokens’ up, I can now sell my ‘Made up Tokens’ at a higher gas fee and make a profit.  


### Being Sandwiched

If you are looking to perform a Swap, at the point when your Swap information is broadcast to the mempool as in a front running attack. an actor can run a transaction in ‘front’ of yours, except they run it at a price that drives the price up for your own trade. They then immediately followup after your own transaction has been processed, with another transaction, this alters the price again and puts their transactions in line to profit at your expense.

This is also referred to as a sandwich attack and utilizes the slippage in price to extract value from the user. 

In general, the probability of a successful mempool malicious attack is higher if the frequency of newblocks added to blockchains is lower.


## How to retrieve pending transactions from the mempool

There are a number of ways to retrieve pending transactions using websockets, Geth Client and GraphQL

* Websocket Subscriptions
* Txpool API
* GraphQL API

### Understanding Pending Transaction Types

* Global pending transactions refer to pending transactions that are happening globally, which includes your newly created local pending transactions.

* Local pending transactions refer strictly to the transactions that you created on your local node. Note that you need ‘personal’ namespace enabled for Geth to send local transactions.

* Pending transactions refer to transactions that are pending due to various reasons, like extremely low gas, out of order nonce, etc.


## 00 Let’s Get Started

### Create a filter using Websockets

Subscribe to Incoming Pending Transactions


```
subscribe("pendingTransactions")
web3.eth.subscribe('pendingTransactions' [, callback]);
```

#### Parameters

`String` - "pendingTransactions", the type of the subscription.

`Function` - (optional) Optional callback, returns an error object as first parameter and the result as second. Will be called for each incoming subscription.

#### Returns

`EventEmitter` -  A subscription instance as an event emitter with the following events:

`data` returns String: Fires on each incoming pending transaction and returns the transaction hash.

`error` returns Object: Fires when an error in the subscription occurs.

Notification returns

Object|Null - First parameter is an error object if the subscription failed.

String - Second parameter is the transaction hash.

Example


```
var subscription = web3.eth.subscribe('pendingTransactions', function(error, result){
    if (!error)
        console.log(result);
})
.on("data", function(transaction){
    console.log(transaction);
});

// unsubscribes the subscription
subscription.unsubscribe(function(error, success){
    if(success)
        console.log('Successfully unsubscribed!');
});
```


Create filter using Web3.py

The web3.eth.Eth.filter() method can be used to set up filters for pending transactions:  \


web3.eth.filter('pending')


```
pending_transaction_filter= web3.eth.filter('pending')
```


Retrieve filtered data


```
print(pending_transaction_filter.get_new_entries())
```



## TX Pool API

The Txpool API gives you access to several non-standard RPC methods to inspect the contents of the transaction pool containing all the currently pending transactions as well as the ones queued for future processing. Txpool methods generally give less precise information as it takes the node time to collect it and prepare a reply.


### Txpool_content

This call returns the transactions contained within the transaction pool. 

The content inspection property can be queried to list the exact details of all the transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only.

The result is an object with two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with actual transactions.

Please note, there may be multiple transactions associated with the same account and nonce. This can happen if the user broadcasts multiple ones with varying gas allowances (or even completely different transactions).

Params (0)

None

Result

RPC
 {"method": "txpool_content"}


Example:


```
curl -X POST -H "Content-Type: application/json" http://localhost:8545 --data '{"jsonrpc": "2.0", "id": 42, "method": "txpool_content", "params": []}'
```



## txpool_inspect

Inspect retrieves the content of the transaction pool and flattens it into an easily inspectable list.

The inspect inspection property can be queried to list a textual summary of all the transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only. This is a method specifically tailored to developers to quickly see the transactions in the pool and find any potential issues.

The result is an object with two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with transaction summary strings.

Please note, there may be multiple transactions associated with the same account and nonce. This can happen if the user broadcast multiple ones with varying gas allowances (or even completely different transactions).

RPC
{"method": "txpool_inspect"}


Params (0)None

Result


