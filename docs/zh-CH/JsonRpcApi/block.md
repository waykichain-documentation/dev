<extoc></extoc>

# 区块相关 API
---


## getblockcount
获取当前节点区块高度

**Parameters**

none

**Returns**

`result` 当前区块高度

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
查询区块哈希值

**Parameters**

`height`  区块高度 **0 代表创世区块**

**Returns**

`hash`  区块哈希

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
获取区块链相关信息

**Parameters**

none

**Returns**

`chain` main:主网、testnet:测试网、regtest:私有链

`blocks` 当前节点最新高度

`bestblockhash` 当前节点最新区块哈希

`verificationprogress` 验证进度

`chainwork` 链的工作量

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
根据区块高度/区块哈希获取区块信息

**Parameters**

`height`/`hash` 区块高度/区块哈希

**Returns**

`hash` 区块哈希

`confirmations` 区块确认数

`size` 区块大小,单位是byte

`height` 区块高度

`version` 区块版本

`merkleroot` merkle root值

`txnumber` 交易数

`tx`, 交易哈希

`time` 区块生成时间戳，单位为距离1970.1.1的时间秒数

`nonce` 随机数

`previousblockhash` 父区块哈希

`nextblockhash` 下一个区块哈希

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
        "previousblockhash":"b4bf3e6238ecaea8f616074481851fd6e18858c93dfdc16f28ee9c9075d0ea13",
        "nextblockhash":"f2c2aa3491619bc8985c7398b6695919443d97db2e38d672240a1f16a94cd437"
    },
    "error":null,
    "id":"curltest"}

```

---



## getinfo
获取节点相关信息

**Parameters**
* none

**Returns**

`version` 节点版本号

`fullversion`   节点的编译版本号

`protocolversion` 协议版本号

`walletversion`  钱包版本号

`balance`   钱包中的总余额

`timeoffset`   分叉时间

`proxy` `host:port`  节点代理地址:端口

`nettype`    网络类型

`chainwork`    链的工作量

`tipblocktime`  最新区块产生时间

`paytxfee` : `x.xxxx`   交易费fee

`relayfee`: `x.xxxx`    交易费fee的最小值

`fuelrate`

`fuel`

`data directory`  节点数据保存路径

`tip block hash` 最新区块hash值

`syncheight` 网络中最新区块高度

`blocks` 当前节点中最新区块高度,

`connections`   当前连接数

`errors` 错误信息

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getinfo","params":[]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result": {
        "version": 1010101,
        "fullversion": "v1.1.1.1-0e4b054-release-linux (2019-01-23 10:08:50 +0800)",
        "protocolversion": 10001,
        "walletversion": 0,
        "balance": 207900000,
        "timeoffset": 0,
        "proxy": "",
        "nettype": "REGTEST_NET",
        "genblock": 1,
        "chainwork": "000000000000000000000000000000000000000000000000000000000000014c",
        "tipblocktime": 1548227299,
        "paytxfee": 0.0001,
        "relayfee": 0.00001,
        "fuelrate": 1,
        "fuel": 0,
        "data directory": "/root/.WaykiChain/regtest",
        "tip block hash": "d1211d634ad22a1640ff1df179380d796fd135079e414d4f05f23db9a182bee3",
        "syncheight": 332,
        "blocks": 332,
        "connections": 0,
        "errors": ""
    },
    "error": null,
    "id": "curltext"
}
```
