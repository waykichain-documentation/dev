
<extoc></extoc>

# 用户地址相关 API

---


## listaddr
查询当前节点地址列表信息

**Parameters**

none

**Returns**

`addr` 地址

`balance` 余额

`haveminerkey` 矿工标识 (可忽略)

`regid` 账户对应的regid

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"listaddr","params":[]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  [
    {
      "addr":"WR4fyUnQKhrxgXuMnjMCea6TNjK29GgHnY",
      "balance":0.00000000,
      "haveminerkey":false,
      "regid":" "
    },
    {
      "addr":"WWnVCpn9j1udKcXFeHzPs55FL7yy7LYDSq",
      "balance":0.00000000,
      "haveminerkey":false,
      "regid":" "
    }
  ],
  "error":null,
  "id":"curltext"
}
```

---


## getnewaddress
创建新地址

**Parameters**

none

**Returns**

`addr` 维基币地址

`minerpubkey` 旷工pubkey,如果没有旷工pubkey，则为no

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getnewaddress"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "addr":"wb5QZWr7PpTsoL6wBYsojfM3G4yaD5x1yA",
    "minerpubkey":"no"
  },
  "error":null,
  "id":174633286
}
```

---


## registeraccounttx
激活账户,新建的地址必须激活后才能作为交易发起方

**NOTE**：`registeraccounttx`、`registaccounttx`同时向后兼容

**Requires unlocked wallet**

Y

**Parameters**

`address` 需要激活的地址

`fee` 激活使用的小费 **单位为`sawi`**,最小 **10000 sawi**


**Returns**

`hash` 交易hash

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"registeraccounttx","params":["WR4fyUnQKhrxgXuMnjMCea6TNjK29GgHnY",10000]}' -H 'content-type:application/json;' http://127.0.0.1:6967
// Or
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"registaccounttx","params":["WR4fyUnQKhrxgXuMnjMCea6TNjK29GgHnY",10000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "hash" : "b93c02997cb71210b9b25969247416a7d31bd803526078f247db4624c14ed340"
  },
  "error":null,
  "id":"curltext"
}
```


---


## genregisteraccountraw
创建激活账户交易的签名，可用 [submittx](./tx.md/#submittx) 方法在区块链上进行广播


**Parameters**

`fee` 激活使用的小费，**单位为`sawi`**

`height` 激活时的区块高度

`publickey` 激活账户的公钥

**Returns**

`rawtx` 激活账户交易的签名数据

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"genregisteraccountraw","params":[1000000,317475,"02a252336665d54160ac16eb83c3e57d254cd6abaab84c6f752f7650a399f0cc31"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":{
    "rawtx":"020192af232102a252336665d54160ac16eb83c3e57d254cd6abaab84c6f752f7650a399f0cc3100bc8340463044022001b7a2acc50d71a5281f28f14804228739e23406599dfcb8125e6f8633f9fe3a02201c73f6c659d8f9e9f6915fbd33ec81103e91d9a5d6372a9288f1f837841a9b39"
    },
  "error":null,
  "id":"curltext"
}

```


---


## getaccountinfo
获取普通账户或者合约账户地址详情

**Parameters**

`address`/`regid` 普通账户/合约账户的地址/regid

**Returns**

`Address` 普通账户/合约账户地址

`KeyID` 地址公钥两次sha之后得到的pubKeyHash

`RegID` 地址对应的regid

`PublicKey` 地址对应的公钥，只有普通账户地址有

`MinerPKey` 矿工标识 (可忽略)

`Balance` 地址的余额,单位为`sawi`

`Votes` 投票数量（涉及Dpos投票的参数）

`UpdateHeight` (可忽略)

`voteFundList` 投票列表

`position` 所在位置; `inwallet`:没有收到过币，`inblock`:收到过币

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","method":"getaccountinfo","params":["wTMrkcgbfFWEWQxBs1XHvGgyTkd5JenQxP"],"id":440924659}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "Address":"wTMrkcgbfFWEWQxBs1XHvGgyTkd5JenQxP",
    "KeyID":"54cf13450d208867450785fca1ccba78813a9028",
    "RegID":"131232-1",
    "PublicKey":"02dae8a12b4b02da6d8bad5d2b04ac23fa8709fbf1016abb4cbed8303a695ab4d5",
    "MinerPKey":"",
    "Balance":499889929,
    "Votes":0,
    "UpdateHeight":0,
    "voteFundList":[],
    "position":"inblock"
  },
  "error":null,
  "id":440924659
}

```


---


## dumpprivkey
获取地址对应的私钥

**Parameters**

`address` 本钱包里普通账户地址

**Returns**

`privatekey` 私钥（WIF格式）

`minerkey`   旷工标识（可忽略）

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"dumpprivkey","params":["WU43D2KLnj1msgrpBeJ5xukR1toD7WTjpS"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "privkey":"PibUXsLW6jyixtuxUTDhwDm6q5FDXkd6JAevDmy5soLFPxxxxxx",
    "minerkey":" "
  },
  "error": null,
  "id": "curltext"
}
```

---


## importprivkey
将私钥（由dumpprivkey导出）导入钱包

**Require**

钱包必须是解锁状态

**Parameters**

`privkey` 普通账户地址私钥（WIF格式）

**Returns**

`imorpt key address` 普通账户地址

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"importprivkey","params":["PibUXsLW6jyixtuxUTDhwDm6q5FDXkd6JAevDmy5soLFPxxxxxx"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "imorpt key address":"WU43D2KLnj1msgrpBeJ5xukR1toD7WTjpS"
  },
  "error": null,
  "id": "curltext"
}

```


---


## validateaddr
检查普通地址或者合约地址是否有效

**Parameters**

`address` 普通账户/合约账户的地址/regid

**Returns**

`ret`= `true`：有效的

`ret`= `false` 无效的

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"validateaddr","params":["wVCfTim2m7F6u5qt9SUixUngbqkpGMoSL6"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "ret":true
  },
  "error": null,
  "id": "curltext"
}
```


---
