---
title: Binance API
id: bsc-api
---

# Binance Smart Chain API


## Develop on Binance Smart Chain (BSC)

In addition to Ankr’s free and public [Binance Smart Chain RPC](https://rpc.ankr.com/bsc), Ankr allows users to create their own full and archive APIs with a variety of options for request call limits, archived data, and more. Ankr’s novel cluster technology allows APIs to draw from multiple nodes, offering a more reliable experience for our users.&#x20;

## Get started on Binance Smart Chain

1. Login or set up an account on &#x20; [app.ankr.com](https://app.ankr.com/api/)
2. [**Create API**](https://app.ankr.com/apps/api)

## Node Types Available on Ankr

* BSC Full
* BSC Archive

## Network Types Available on Ankr

* Mainnet - Full & Archive
* Testnet - Full

## Explorer Links <a href="#explorer-links" id="explorer-links"></a>

Mainnet - [https://bscscan.com/](https://bscscan.com)​

Testnet - [https://testnet.bscscan.com/](https://testnet.bscscan.com)​

### Endpoints Available

* JSON RPC API;
* Websockets;

## Examples

#### API (HTTPS) Endpoint

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="go" label="Go">

```go
package main

import (
    "context"
    "fmt"
    "github.com/ethereum/go-ethereum/ethclient"
)
func main() {
    const url_auth = "https://username:password@apis.ankr.com/xxxxx/xxxxx/binance/full/main"    // authentication
    const url_token = "https://apis.ankr.com/xxxxx/xxxxx/binance/full/main"                     // token
    
    rpcClient,err := ethclient.Dial("choose url_auth or url_token by your created type")
    
    if err != nil {
        panic(err)
    }
    
    blockNumber, err := rpcClient.BlockNumber(context.Background())
    
    if err != nil {
        panic(err)
    }
    
    fmt.Println(blockNumber)
}
```
</TabItem>
<TabItem value="js" label="JavaScript">

```javascript
const Web3 = require('web3');
const url_auth = 'https://username:password@apis.ankr.com/xxxxx/xxxxx/binance/full/main'    // authentication
const url_token = 'https://apis.ankr.com/xxxxx/xxxxx/binance/full/main'                     // token
const web3 = new Web3(new Web3.providers.HttpProvider('choose url_auth or url_token by your created type'));

web3.eth.getBlockNumber((error, blockNumber) => {
    if(!error){
        console.log(blockNumber);
    }else{
        console.log(error);
    }
})
```
</TabItem>
<TabItem value="py" label="Python">

```python
from web3 import Web3

def test_block_number(self):
    url_auth = 'https://username:password@apis.ankr.com/xxxxx/xxxxx/binance/full/main'  # authentication
    url_token = 'https://apis.ankr.com/xxxxx/xxxxx/binance/full/main'                   # token
    
    web3 = Web3(HTTPProvider('choose url_auth or url_token by your created type'))
    print(web3.eth.block_number)
```
</TabItem>
<TabItem value="curl" label="Curl">

```bash
# authentication
$ curl -H "Content-Type: application/json" -u "username:password" -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' https://apis.ankr.com/xxxxx/xxxxx/binance/full/main
$ curl -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' https://username:password@apis.ankr.com/xxxxx/xxxxx/binance/full/main

# token
$ curl -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' https://apis.ankr.com/xxxxx/xxxxx/binance/full/main
```
</TabItem>
<TabItem value="truffle" label="truffle">

```java
    const fs = require('fs');
    const mnemonic = fs.readFileSync(".secret").toString().trim();//.secret   Mnemonic Phrase
    var HDWalletProvider = require("truffle-hdwallet-provider");
    require('babel-register');
    networkName: {
        provider: () => new HDWalletProvider(mnemonic, `https://apis.ankr.com/xxxxx/xxxxx/binance/archive/main`),
        network_id: 56, // Chain id
        gas: 4612388, // Chain has a lower block limit than mainnet
        confirmations: 2, // # of confs to wait between deployments. (default: 0)
        timeoutBlocks: 200, // # of blocks before a deployment times out  (minimum/default: 50)
        skipDryRun: true, // Skip dry run before migrations? (default: false for public nets )
    }
```
</TabItem>
</Tabs>

#### Websocket (WSS) Endpoint

<Tabs>
<TabItem value="go" label="Go">

```go

package main

import (
    "context"
    "fmt"
    "github.com/ethereum/go-ethereum/core/types"
    "github.com/ethereum/go-ethereum/ethclient"
    "time"
)

func main() {
    const url_auth = "wss://username:password@apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main" // authentication

    const url_token = "wss://apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main"                  // token
    
    client, err := ethclient.Dial(`choose url_auth or url_token by your created type`)
    
    if err != nil {
        panic(err)
    }
    
    ch := make(chan *types.Header, 1024)
    sub, err := client.SubscribeNewHead(context.Background(), ch)
    
    if err != nil {
        panic(err)
    }
    
    fmt.Println("---subscribe-----")
    go func() {
        time.Sleep(10 * time.Second)
        fmt.Println("---unsubscribe-----")
        sub.Unsubscribe()
    }()

    go func() {
        for c := range ch {
            fmt.Println(c.Number)
        }
    }()

    <-sub.Err()

}
```
</TabItem>
<TabItem value="ethers.js" label="ethers.js">

```javascript
const ethers = require("ethers");
const url_auth = 'wss://username:password@apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main'    // authentication
const url_token = 'wss://d@apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main'                   // token
const init = function () {
    const wsProvider = new ethers.providers.WebSocketProvider('choose url_auth or url_token by your created type');
    wsProvider.on("pending", (tx) => {
        console.log("tx", tx)
        setTimeout(function () {
            wsProvider.destroy()
        }, 1000);

    });

};

init();
```
</TabItem>
<TabItem value="web3.js" label="web3.js">

```javascript
const WebSocket = require('ws');
const url_auth = 'wss://username:password@apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main'    // authentication
const url_token = 'wss://d@apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main'                   // token
const request = '{"id": 1, "method": "eth_subscribe", "params": ["newPendingTransactions"]}';  
const ws = new WebSocket('choose url_auth or url_token by your created type');

ws.on('open', function open() {
    ws.send(request);
});
ws.on('message', function incoming(data) {
    res = JSON.parse(data)
    if (res.result != null) {
        console.log(`Subscription: ${res.result}`);
    } else if (res.params != null && res.params["result"] != null) {
        console.log(`New pending transaction: ${res.params["result"]}`);
    } else {
        console.log(`Unexpected: ${data}`);
    }
});
```
</TabItem>
<TabItem value="curl" label="Curl">

```bash
# authentication
$ wscat -c wss://username:password@apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main


# token
$ wscat -c wss://apis.ankr.com/wss/xxxxx/xxxxx/binance/full/main

wait connected...

# subscribe
> {"jsonrpc":"2.0","method":"eth_subscribe","params":["newHeads"],"id":1}


# unsubscribe
> {"jsonrpc":"2.0","method":"eth_unsubscribe","params":["0xxxxxxxxxxxxxxx"],"id":1}
```
</TabItem>
<TabItem value="truffle" label="Truffle">

```java
const fs = require('fs');
    const mnemonic = fs.readFileSync(".secret").toString().trim();//.secret   Mnemonic Phrase
    var HDWalletProvider = require("truffle-hdwallet-provider");
    require('babel-register');
    networkName: {
        provider: () => new HDWalletProvider(mnemonic, `wss://apis.ankr.com/xxxxx/xxxxx/binance/archive/main`),
        network_id: 56, // Chain id
        gas: 4612388, // Chain has a lower block limit than mainnet
        confirmations: 2, // # of confs to wait between deployments. (default: 0)
        timeoutBlocks: 200, // # of blocks before a deployment times out  (minimum/default: 50)
        skipDryRun: true, // Skip dry run before migrations? (default: false for public nets )
    }
```
</TabItem>
</Tabs>