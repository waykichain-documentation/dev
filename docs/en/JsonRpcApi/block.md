
<extoc></extoc>

# API about block

---


## getblockcount
Get the number of blocks in the current node

**Parameters** 

none

**Returns**

`result` the number of blocks in the current node

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getblockcount"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":1284292,
  "error": null,
  "id": "curltext"
}
```

---


## getblockhash 
Get the hash of block

**Parameters**

`height`  Block height **0 is the genesis block**

**Returns**

`hash`  block hash

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getblockhash","params":[1284292]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "hash":"c4d4886eb31a54609306362dabdb8e59165957b0afb1900c3cf2234211ff39f7"
    },
    "error":null,
    "id":"curltext"
}
```

---


## getblockchaininfo
Get an object containing various state info regardingblock chain processing.

**Parameters**

none

**Returns**

`chain` main:mainnet、testnet:testnet、regtest:privatenet

`blocks` current node latest height

`bestblockhash` current node latest block hash

`verificationprogress` estimatation of verification progress

`chainwork` total amount of work in active chain, in hexadecimal

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getblockchaininfo"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "chain":"main",
        "blocks":1284655,
        "bestblockhash":"cbfda69d79ec829a399d78242824f5338a6980e850ef323239ca0f6afeace2fa",
        "verificationprogress":1.00000000,
        "chainwork":"0000000000000000000000000000000000000000000000000000000000139a2f"
    },
    "error":null,
    "id":"curltext"
}
```



---


## getblock
Get block information about the block with the given hash orindex.

**Parameters**

`height`/`hash` block height/block hash


**Returns**

`hash` block hash

`confirmations` block confirmations

`size`  The block size, unit is byte

`height`  The block height or index

`version`  The block version

`merkleroot` The merkle root

`txnumber` The transaction number

`tx` The transaction id

`time` The block time in seconds since epoch (Jan 1 1970 GMT)

`nonce` The nonce

`previousblockhash` previous block hash

`nextblockhash` The hash of the next block

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc": "1.0", "id":"curltest", "method": "getblock", "params": [246412] }' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "hash":"f700adb00ef9fa652b242698eae6c3f5066b6f1e8f6cb1f52d8683a6811e48fe",
        "confirmations":11,
        "size":174,
        "height":246412,
        "version":1,
        "merkleroot":"d38e093bec35d86cf0e715689c7a41adeadb06db59169fa082da26dea71448f5",
        "txnumber":1,
        "tx":["d38e093bec35d86cf0e715689c7a41adeadb06db59169fa082da26dea71448f5"],
        "time":1540461840,
        "nonce":191,
        "chainwork":"000000000000000000000000000000000000000000000000000000000003c28c",
        "fuel":0,
        "fuelrate":1,
        "previousblockhash":"b4bf3e6238ecaea8f616074481851fd6e18858c93dfdc16f28ee9c9075d0ea13","nextblockhash":"f2c2aa3491619bc8985c7398b6695919443d97db2e38d672240a1f16a94cd437"
    },
    "error":null,
    "id":"curltest"}

```

---


## getinfo
Get an object containing various state info.

**Parameters**
* none

**Returns**

`version` the node version

`fullversion`   the node build full version

`protocolversion` the protocol version

`walletversion`    the wallet version

`balance`         the total Coin balance of the wallet

`timeoffset`        the time offset

`proxy` `host:port`    the proxy used by the server(optional)

`nettype`            the net type

`chainwork`        the  chain work of the tip block in chainActive

`tipblocktime`        the  nTime of the tip block in chainActive

`paytxfee` : `x.xxxx`     the transaction fee

`relayfee` : `x.xxxx`       minimum relay fee for non-free transactions

`fuelrate`    the  fuelrate of the tip block in chainActive

`fuel`   the  fuel of the tip block in chainActive

`data directory`  the data directory

`tip block hash` the tip block hash

`syncheight` the current blocks height int network

`blocks`             the current node blocks height

`connections`        the number of connections

`errors` any error messages

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getinfo","params":[]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result": {
        "version": 1010001,
        "fullversion": "v1.1.0.1-075db0b-release-linux (2018-12-26 15:47:51 +0800)",
        "protocolversion": 10001,
        "walletversion": 0,
        "balance": 0,
        "timeoffset": 0,
        "proxy": "",
        "nettype": "TEST_NET",
        "chainwork": "000000000000000000000000000000000000000000000000000000000006bd13",
        "tipblocktime": 1544217250,
        "paytxfee": 0.0001,
        "relayfee": 0.00001,
        "fuelrate": 1,
        "fuel": 0,
        "data directory": "/root/.WaykiChain/testnet",
        "tip block hash": "5e8225a5e480eff37125bdbbbd1bde524606d3b5f929515064e48a8fb617c707",
        "sync tip blocks": 694545,
        "received blocks": 441619,
        "connections": 2,
        "errors": ""
    },
    "error": null,
    "id": "curltext"
}
```