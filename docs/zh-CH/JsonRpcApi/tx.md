<extoc></extoc>

# 交易相关 API

---

## sendtoaddress
从源地址账户转账到目的地址账户，手续费默认为10000`sawi`

**Parameters**

`sender` 发送方地址（Optional）

`recviver` 接收方地址

`amount` 发送金额 **单位为`sawi`**，最小金额 **10000 sawi**

**Returns**

`hash` 交易哈希

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","method":"sendtoaddress","params":["wTwrWser78mEa22f8mHfiHGrdKysTv8eBU","wXscdWVUN9irrLGvJJDZiaBoKXvat6Pqa9",1000000000],"id":168141569}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "hash":"c8260a08723192c74fb2025093bb876e6532baa5af952e6f7758a40c7f61c81f"
  },
  "error":null,
  "id":168141569
}
```

---


## sendtoaddresswithfee
从源地址账户转账到目的地址账户,手动设置手续费

**Parameters**

`sender` 发送方地址（Optional）

`recviver` 接收方地址

`amount` 发送金额 **单位为 `sawi`**，最小金额 **10000 sawi**

`fee` 手续费 **单位为`sawi`**，最小 **10000 sawi**

**Returns**

`hash` 交易哈希

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","method":"sendtoaddresswithfee","params":["wTwrWser78mEa22f8mHfiHGrdKysTv8eBU","wXscdWVUN9irrLGvJJDZiaBoKXvat6Pqa9",1000000000,10000],"id":168141569}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "hash":"c8260a08723192c74fb2025093bb876e6532baa5af952e6f7758a40c7f61c81f"
  },
  "error":null,
  "id":168141569
}
```

---


## gensendtoaddressraw
创建交易签名字段,手动设置手续费,可用 `submittx` 方法进行提交交易

**Parameters**

`sender`  发送方地址

`recviver`  接收方地址

`amount` 发送金额 **单位为`sawi`**，最小金额 **10000 sawi**

`fee`   手续费 **单位为`sawi`**，最小 **10000 sawi**

**Returns**

`rawtx` 创建交易签名产生的签名字段

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","method":"gensendtoaddressraw","params":["wLKf2NqwtHk3BfzK5wMDfbKYN1SC3weyR4","wXscdWVUN9irrLGvJJDZiaBoKXvat6Pqa9",100000,10000],"id":403309340}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "rawtx":"03018de95e0480c855010486db2601cd10858c20004630440220733eb4610e5cbb599ef409fd871cb1a999af2743f4d87b2c1f8e792c0c901f3a022000ce48fea9250faf25cc6a4bf19ec098a249a4f89116e2f033e60921c26fef55"
  },
  "error":null,
  "id":403309340
}
```

---


## submittx
将交易签名数据广播至区块链

**Parameters**

`rawtx` 已创建的交易签名字段，可以通过`gensendtoaddressraw`方法生成

**Returns**

`hash` 交易哈希

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"submittx","params":["030192af0f020008146902285df4bba1d69952f3f9e5b071ed3e4e8df4cd10aed6c1000046304402204d3ac6de164f4bdfa65075e54bcd32161fa0ae3745d7b0e3e5da7f79ec17b45802206dd239adbbe5b3cc66adbb138505327036e111283c15e046b1b5fce5d52aa295"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "hash":"72a9ac4e3adb1ad93f7d6da59e61d9bf22918db17c4320555782fdcfe68899f8"
  },
  "error":null,
  "id":"curltext"
}

```


---


## gettxdetail
根据交易哈希查询交易详情,包括已确认和未确认的交易

**Parameters**

`txhash` 交易哈希

**Returns**

`hash` 交易哈希

`txtype` 交易类型
   * REG_ACCT_TX：用户激活交易
   * COMMON_TX：WICC转账交易
   * REG_CONT_TX：合约发布
   * CONTRACT_TX：合约调用交易
   * REWARD_TX：矿工奖励
   * DELEGATE_TX: 投票

`ver` 区块版本号

`regid` 发送方地址的激活id

`addr`  发送方地址

`desregid` 接受方地址的激活id

`desaddr` 接受方地址

`money` 交易中转账WICC金额，单位为`sawi`

`fees` 交易中消耗的小费，单位为`sawi`

`height` 发送该交易时的区块高度

`arguments` 调用合约的参数，仅当该交易为合约交易时才会有值

`blockhash` 区块hash

`confirmHeight` 该交易被旷工确认时的区块高度

`confirmedtime` 该交易被旷工确认时的时间戳

`rawtx` 该交易的签名字段

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"gettxdetail","params":["27ab6bc6b1761d208d255564073ada97aab86c8a5259f7b3def3c92c6925e2ef"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result": {
    "hash": "27ab6bc6b1761d208d255564073ada97aab86c8a5259f7b3def3c92c6925e2ef",
    "txtype": "COMMON_TX",
    "ver": 1,
    "regid": "19396-1",
    "addr": "We4jaf5wSFYdFUaxzK5dKGoC2YPb9ATUPB",
    "desregid": " ",
    "desaddr": "WR4fyUnQKhrxgXuMnjMCea6TNjK29GgHnY",
    "money": 100000000,
    "fees": 10000,
    "height": 27991,
    "arguments": "",
    "blockhash": "c37f3e9869c6f8e7a73a3f05225f3190bbbdfe95d3e5472b306e24337b164ad6",
    "confirmHeight": 27994,
    "confirmedtime": 1525918120,
    "rawtx": "030180d9570480964401141a385e61ce1ffb4d9e9adc1d70e7a493adee3dd9cd10aed6c100004630440220422e3b013e9c6096efec06e5187f3afada5ad8537af251078b1923ec14efc6c80220147bd6d8e79ecaf94090d37f07f8f9458d8018a584f59782055cfa7fd34e8556"
  },
  "error": null,
  "id": "curltext"
}
```


---


## listtx
获取当前节点交易列表：包含已确认和未确认的交易

**Parameters**

 none

**Returns**

`ConfirmTx` 已确认的交易哈希列表

`UnConfirmTx` 未确认的交易哈希列表

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"listtx"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "ConfirmTx":
        [
          "9b0626c3c2d248ad6559e4ce419cb27f35d80a226351cc00e4e10ec5438e1b70",
          "5ad2939094020cd58247b6d93620b5442ac3635e1b0a406bb52a18021241c4e2",
          "be21c999f2273cb0413a5a05f148d1271eca23b3333078c11e07537b1f0a2bf2",
          "d7036ba89bc441b16896b329fb141b478c199cc5708de44e4209aa6fc16efa16",
          "37c31dec529d07c4bffbd70670fc245032301460a1c946f9ebe897c4b6fbf937",
          "c6ac8b3b1bfd70bf9160a2dddb194435d00d47d8727f47b350aa5a4c80e4ed56",
          "7cef103b31f07dbb0c88c06e59d2becc0f29636377f3b1083f39b646b2185390",
          "0702b6c9cc96287b1226558eee08b114a3f99b788e2c61346416175d4b418a9b",
          "6b82eaa22da56afafeee3df1807974f36600d7181c1e9e703fa4c9d77046549f",
          "f1aced4bcfc914d0d0cde1fa98cb851c20e537df444b9161f57fa07d50a933cc"
        ],
        "UnConfirmTx":[]
    },
    "error":null,
    "id":"curltext"
}

```


---


## listunconfirmedtx
获取当前节点未被确认的交易列表

**Parameters**
* none

**Returns**

`UnConfirmTx` 当前节点未被确认的交易列表

    *`txType=` 交易类型,详见`gettxdetail`
    *`hash`  交易hash
    *`ver`
    *`srcId` 发送方regid
    *`desId` 接收方regid
    *`llFees` 交易手续费
    *`vContract` 合约内容，合约交易时才有
    *`nValidHeight`  提交交易时的区块高度

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"listunconfirmedtx"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "UnConfirmTx":
    [
      "txType=COMMON_TX, hash=cca0868ce56515f3929296e393b2b7151b5baf7e2ee5828889378b2da3c92549, ver=1, srcId=138259-1 desId=185091-1, llFees=10000, vContract=, nValidHeight=283078\n"
    ]
  },
  "error":null,
  "id":"curltext"
}

```


---


## getalltxinfo
获取交易详情，包含已确认和未确认的交易


**Parameters**

`txcount`  交易记录数量（Optional）,不填时查看所有交易详情

**Returns**

`Confirmed`  已确认的交易信息数组

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getalltxinfo","params":[2]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "Confirmed":
    [
      {
        "hash":"cca0868ce56515f3929296e393b2b7151b5baf7e2ee5828889378b2da3c92549",
        "txtype":"COMMON_TX",
        "ver":1,
        "regid":"138259-1",
        "addr":"wQWKaN4n7cr1HLqXY3eX65rdQMAL5R34k6",
        "desregid":"185091-1",
        "desaddr":"web3Xj77ShBZJjQ54jzttZpAfdB8BAYn7M",
        "money":100000000,
        "fees":10000,
        "height":283078,
        "Contract":"",
        "blockhash":"c99e077ce23524cc8bb7dbdc24f75e611a85c9f91efe91fe6d7972187ef48a25",
        "confirmHeight":283079,
        "confirmedtime":1541216320,"rawtx":"030190a2460487b71301048aa50301cd10aed6c10000473045022100917610f3f7c271c3e1435238a316d38c9f58c0f3bbc541ea9ae51c7442e263fd022042a18a39857cc912c5ebda2962fa23747605d3f2a326dbd35a905629bcb63ba9"
      },
      {
        "hash":"f36d5058d28769e413f905c6604ac79de8ed3799fc6fca678bb76cbb74d51a69",
        "txtype":"COMMON_TX",
        "ver":1,
        "regid":"138259-1",
        "addr":"wQWKaN4n7cr1HLqXY3eX65rdQMAL5R34k6",
        "desregid":"185091-1",
        "desaddr":"web3Xj77ShBZJjQ54jzttZpAfdB8BAYn7M",
        "money":100000000,
        "fees":10000,
        "height":282993,
        "Contract":"",
        "blockhash":"230eee735caf9a6abccdb2652e8b5000e76ba504d79ccbb7de69fa22f82e9db0",
        "confirmHeight":282995,
        "confirmedtime":1541214790,"rawtx":"030190a1710487b71301048aa50301cd10aed6c10000473045022100c4f65991e9ba08a4dfd0792b1a792f7e534dc40e07426a9abc49ba6b206430ba022058cbd26bd5fc82e701632d6ccbc4b025e65e21a6cd9c21cdaa3550088f28c704"
      }
    ]
  },
  "error":null,
  "id":"curltext"
}
```


## decoderawtx
根据签名字段解析原始交易单

**Parameters**

`rawtx` 签名后的交易字段

**Returns**

`hash` 交易hash

`txtype` 交易类型

`ver` 版本

`regid` 交易sender的regid

`addr` 交易sender的地址

`desregid` 交易receiver的regid

`desaddr` 交易receiver的地址

`money`  交易金额 ，**单位为 `sawi`**

`fees`  手续费，**单位为 `sawi`**

`height` 交易发送签名时的高度

`contract` 合约内容

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc": "1.0", "id":"curltest", "method": "decoderawtx", "params": ["0301d60002000114865044b3ac4f7e1facb0f881666ef557d441e0ffcd10858c20004630440220656c7badc84d3625139bd0935200a0ba3ee763ae9fce76c64457785e1c46470302200590c2941168828305514d978261456eb82c46bd390624c9bccdaabee9894252"] }' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result": {
        "hash": "6cab7e9b4b6e25f2390de3087f1024adf1f430c2aedab9eed6d82d33786436da",
        "txtype": "COMMON_TX",
        "ver": 1,
        "regid": "0-1",
        "addr": "wLKf2NqwtHk3BfzK5wMDfbKYN1SC3weyR4",
        "desregid": " ",
        "desaddr": "wXscdWVUN9irrLGvJJDZiaBoKXvat6Pqa9",
        "money": 100000,
        "fees": 10000,
        "height": 11136,
        "contract": ""
    },
    "error": null,
    "id": "curltest"
}
```
