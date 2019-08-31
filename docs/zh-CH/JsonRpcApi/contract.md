<extoc></extoc>

# 智能合约相关 API

---

## registercontracttx
用于部署智能合约
**任何地址都可以发布智能合约，智能合约发布者不一定是合约的拥有者。**

**被部署的智能合约文件必须在节点运行环境的`/tmp/lua/`路径下**

**Parameters**

`address` 发布智能合约者的地址

`filepath` 发布的智能合约文件路径+文件名

`fee` 发布智能合约所需费用 （一般至少110000000）

**Returns**

`hash` 发布合约交易的哈希

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"registercontracttx","params":["wd3hLkmd5Jrmck4Rsg8WCcJ3PfFrKFDbbA","/tmp/lua/lotteryV3.lua",110000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "hash":"641794c5db4af756660197878dce462b887224a2efad8387d915bbc2acb5aa9d"
    },
    "error":null,
    "id":"curltext"
}
```


---


## getcontractregid
获取智能合约的regid

**Parameters**

`txhash` 发布合约时的交易哈希

**Returns**

`regid`   该智能合约的regid

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractregid","params":["641794c5db4af756660197878dce462b887224a2efad8387d915bbc2acb5aa9d"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "regid:":"320561-1",
        "regid_hex" : "31e404000100"
    },
    "error":null,
    "id":"curltext"
}
```


---


## getcontractaccountinfo
获取用户在智能合约中的相关信息

**Parameters**

`regid` 智能合约的regid

`address`  查询的用户地址/regid

**Returns**

`mAccUserID` 查询的用户地址转成16进制字符串

`FreeValues` 智能合约的自由可操作的币的数量

`vFreezedFund` 智能合约的冻结的币的数量和解冻时间的数据列表

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractaccountinfo","params":["314219-2","wd3hLkmd5Jrmck4Rsg8WCcJ3PfFrKFDbbA"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "mAccUserID":"776433684c6b6d64354a726d636b34527367385743634a33506646724b4644626241",
        "FreeValues":0,
        "vFreezedFund":[]
    },
    "error":null,
    "id":"curltext"
}
```


---


## callcontracttx
创建调用智能合约交易,合约必须是被确认状态

**Parameters**

`userregid` 合约调用者地址/regid

`regid`    智能合约的regid

`amount`   向智能合约发送维基币的数量,单位为`sawi`

`arguments`  调用合约的参数

`fee`, 调用合约交易手续费, 最小 **100000 sawi**

**Returns**

`hash` 调用合约的交易哈希

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"callcontracttx","params":["wbj4NRzW5TPD9LvckchYiJPDbD9esdZPnp","314219-2",0,"f00657695a7836727273426e3973486a7770766477744d4e4e58326f3331733344454848000052acdfb2241d000052acdfb2241d0100000001000000",1000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
       "hash":"137d795f5dbe29a226a60054d85868a62fca15b7ad43ce35e79ebf18dbcee99a"
    },
    "error":null,
    "id":"curltext"
}

```




---


## getcontractdata
获取智能合约相关原生数据信息

**Parameters**

`regid` 合约regid

`key`   智能合约数据的key值


**Returns**

`regid`  智能合约的regid

`key`  智能合约数据的key值

`value` 智能合约数据的value值

**Example**

```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractdata","params":["67795-1","admin"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "regid":"67795-1",
        "key":"admin",
        "value":"wTwrWser78mEa22f8mHfiHGrdKysTv8eBU"
    },
    "error":null,
    "id":"curltext"
}

```
---


## getcontractdataraw
获取智能合约相关数据信息

**Parameters**

`regid` 合约regid

`key` 智能合约数据的key值，字符的ASCII码转16进制


**Returns**

`regid` 智能合约的regid

`key` 智能合约数据的key值，字符的ASCII码转16进制

`value` 智能合约数据的value值, 字符的ASCII码转16进制

**Example**

```json
// Request  admin的ASCII码转16进制后得到， 61646d696e
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractdataraw","params":["67795-1","61646d696e"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
      "regid":"67795-1",
      "key":"61646d696e",
      "value":"775477725773657237386d4561323266386d486669484772644b7973547638654255"
    },
    "error":null,
    "id":"curltext"
}

```
---

## listcontracts
获取已发布的智能合约列表

**Parameters**

`showDetail` 是否查询合约内容，`true` 或者 `false`

**Returns**

`scriptId` 智能合约的regid

`description`智能合约的描述

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"listcontracts","params":[false]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "listregedscript":
        [
            {
                "scriptId":"275970-1",
                "description":""
            },
            {
                "scriptId":"148998-1",
                "description":""
            },
            {
                "scriptId":"275082-1",
                "description":"3c73637269707420747970653d22746578742f6a617661736372697074223e616c657274282248656c6c6f205761796b69636861696e2122293c2f7363726970743e"
            }
            {
                "scriptId":"44531-1",
                "description":""
            },
            {
                "scriptId":"157694-1",
                "description":""
            }
        ]
    },
    "error":null,
    "id":"curltext"
}

```

---
## getcontractinfo
获取智能合约信息

**Parameters**

`regid` 智能合约 regid

**Returns**

`scriptId` 智能合约 regid

`description` 智能合约描述

`scriptContent` 智能合约详情

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractinfo","params":["9243-1"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "scriptId" : "9243-1",
    "description" : "",
    "scriptContent" : "6d796c6962203d207265717569726520282225d292e2e27206e6f7420666f756e6427290a20202020656e640a656e640a0a4d61696e28290a0a"
}
```
