
<extoc></extoc>

# 隐藏的 API 
---
## getbalance
查询地址余额

**Parameters**

`address` 本钱包里维基币地址 (Optional, 若不填写则为本钱包内所有余额)

**Returns**

`Balance` 维基币地址的余额,(该值为实际金额，没有乘以10的8次方)

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getbalance","params":["WR4fyUnQKhrxgXuMnjMCea6TNjK29GgHnY"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "balance": 1.00000000
  },
  "error": null,
  "id": "curltext"
}

```

**Comment**

该方法只能查询本钱包地址，而且该地址必须要有金额转入的记录时才能查询， 查询账号信息时尽量使用 getaccountinfo 方法
---

## block.md
---
1、## addnode
添加、删除、连接节点

**Parameters**

`node` 节点的IP和端口号

`command` 命令

| 命令 | 说明 |
| :---   | :-----:                           |
| add    | 向addnode列表添加节点            |
| remove | 从addnode列表中删除节点       |
| onetry | 尝试和目标节点建立一次链接 |

**Returns**

none

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"addnode","params":["47.92.143.137:18920","add"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":null,
  "error":null,
  "id":"curltext"
}
```
---


##投票

---
1、## getdelegatelist
查询BP节点列表

**Parameters**

`num`  需要查询BP节点的数目，范围`1-11`

**Returns**

`Address` BP节点地址

`KeyID` keypubhash

`RegID` BP节点地址 regid

`PublicKey` BP节点地址公钥

`MinerPKey`

`Balance` 地址余额

`Votes` 投票数

`UpdateHeight`

`voteFundList` 投票详情

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc": "1.0", "id":"curltest", "method": "getdelegatelist", "params": [11] }' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result": {
        "delegates": [
            {
                "Address": "wNDue1jHcgRSioSDL4o1AzXz3D72gCMkP6",
                "KeyID": "1c758724cc60db35dd387bcf619a478ec3c065f2",
                "RegID": "0-2",
                "PublicKey": "0376de6a21f63c35a053c849a339598016a0261d6bdc5567adeda0af78b750c4cc",
                "MinerPKey": "",
                "Balance": 949870000,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wNuJM44FPC5NxearNLP98pg295VqP7hsqu",
                "KeyID": "23e8b4ac5e3dea621474cad9d9dc4323018856c9",
                "RegID": "0-3",
                "PublicKey": "025a37cb6ec9f63bb17e562865e006f0bafa9afbd8a846bd87fc8ff9e35db1252e",
                "MinerPKey": "",
                "Balance": 130090000,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wP64X59EoRmeq2M5GrJ23UVttE9uxnuoFa",
                "KeyID": "25f1bd29a3d025fb7e2aa596d86c41e245ddb2c0",
                "RegID": "0-4",
                "PublicKey": "03f52925f191c77bb1d16b19387bcfcb83380f1622d643a11038cf4867c4578696",
                "MinerPKey": "",
                "Balance": 131260006,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wQewSbKL5kAfpwnrivSiCcaiFffgNva4uB",
                "KeyID": "3721af044c38b62b14b18a78339638097d57d830",
                "RegID": "0-5",
                "PublicKey": "0378f1d7ce11bace8bbf28e124cd15f1bc82d7e8a25f62713f812201e1cf8060d8",
                "MinerPKey": "",
                "Balance": 141030996,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wQquTWgzNzLtjUV4Du57p9YAEGdKvgXs9t",
                "KeyID": "39349a5c128882c0fc6034768b95ad8791b213e7",
                "RegID": "0-6",
                "PublicKey": "03fb1fe453bf830843fd90f0d2ae1b67011c168a0a5f2160e41f1d86a64c86c25c",
                "MinerPKey": "",
                "Balance": 150000000,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wRQwgYkPNe1oX9Ts3cfuQ4KerqiV2e8gqM",
                "KeyID": "3f742fd45c7aea013ff97a07574576ed9048819c",
                "RegID": "0-7",
                "PublicKey": "0282464694b94780b88c5e88ff64a7e24800590d4061b90f334067d15643604057",
                "MinerPKey": "",
                "Balance": 34009999,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wSjMDgKWHC2MzrUamhJtyyR2FTtw8oMUfx",
                "KeyID": "4de772892db26d235febe1f6c004e138fb08206c",
                "RegID": "0-8",
                "PublicKey": "02361884b20bdb2f751c56783a5fdc01ec64e25d77b7bf5a30409aef4b5b3b44ce",
                "MinerPKey": "",
                "Balance": 3639950000,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wSms4pZnNe7bxjouLxUXQLowc7JqtNps94",
                "KeyID": "4e6131d0301855593319ad2d00062fc1c95235f9",
                "RegID": "0-9",
                "PublicKey": "029699e9b4d679d04d8961bf64ffc0b5ec6f9d46e88bbdcbc6d0a02ce1e4991c0c",
                "MinerPKey": "",
                "Balance": 21099996,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wT75mYY9C8xgqVgXquBmEfRmAXPDpJHU62",
                "KeyID": "5203b378d7f1b2b9b451bc47742f4a9d96624583",
                "RegID": "0-10",
                "PublicKey": "024af74c1cc6b1d729b038427ce9011627f6b816e77699421a85265c4ae0b74b5b",
                "MinerPKey": "",
                "Balance": 100000000,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wUt89R4bjD3Ca6Vb7mk18oGsVtSTCxJu2q",
                "KeyID": "658087881805a66c7faeb7b942d1930d9bdc72f1",
                "RegID": "0-11",
                "PublicKey": "03e44dcf2df8ec33a17c63a894f3697ed863b2d1fcc7b5fd37cc4fc2edf8e7ed71",
                "MinerPKey": "",
                "Balance": 12019894,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            },
            {
                "Address": "wVTUdfEaeAAVSuXKrmMyqQXH5j5Z9oGmTt",
                "KeyID": "6bcf5b9a38cb8ca656b5c194f2f3c155be93b3ab",
                "RegID": "0-12",
                "PublicKey": "03ff9fb0c58b6097bc944592faee68fbdb2d1c5cd901f6eae9198bd8b31a1e6f5e",
                "MinerPKey": "",
                "Balance": 30840097,
                "Votes": 210000000000000,
                "UpdateHeight": 0,
                "voteFundList": []
            }
        ]
    },
    "error": null,
    "id": "curltest"
}
```
---

## getappdata
获取智能合约相关数据信息

< regid > < key >  or
< regid >< pagsize >[index]


**Parameters case1**

`regid` 合约regid

`key` 智能合约数据的key值，字符的ASCII码转16进制


**Returns case1**

`regid` 智能合约的regid

`key` 智能合约数据的key值，字符的ASCII码转16进制

`value` 智能合约数据的value值, 字符的ASCII码转16进制

**Example**

```json
// Request  admin的ASCII码转16进制后得到， 61646d696e
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getappdata","params":["67795-1","61646d696e"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

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

**Parameters case2**

`regid` 合约regid

`pagesize`

`index`

**Returns case2**

`key`: `value` 智能合约key-value对的数据

**Example**

```json
// Request  admin的ASCII码转16进制后得到， 61646d696e
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getappdata","params":["67795-1",1,1]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    [
        {
            "key":"61646d696e",
            "value":"775477725773657237386d4561323266386d486669484772644b7973547638654255"
        },
        {
            "key":"65786368616e676552617465",
            "value":"fc3a000000000000"
        }
    ],
    "error":null,
    "id":"curltext"
}

```


---


## getappdataraw
获取智能合约相关原生数据信息

< regid > < key >  or

< regid >< pagsize >[index]


**Parameters case1**

`regid` 合约regid

`key`   智能合约数据的key值


**Returns case1**

`regid`  智能合约的regid

`key`  智能合约数据的key值

`value` 智能合约数据的value值

**Example**

```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getappdataraw","params":["67795-1","admin"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

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

**Parameters case2**

`regid` 合约regid

`pagesize`

`index`

**Returns case2**

`key`: `value` 智能合约key-value对的数据

**Example**

```json
// Request  admin的ASCII码转16进制后得到， 61646d696e
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getappdataraw","params":["67795-1",1,1]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    [
        {
            "key":"admin",
            "value":"wTwrWser78mEa22f8mHfiHGrdKysTv8eBU"
        },
        {
            "key":"exchangeRate",
            "value":"\u00FC:\u0000\u0000\u0000\u0000\u0000\u0000"
        }
    ],
    "error":null,
    "id":"curltext"
}
```


---

## islocked
检查钱包是否是锁住状态

**Parameters**

none

**Returns**

`islocked`  钱包是否解锁状态码

| value |                  meaning                  |
|:-----:|:-----------------------------------------:|
|   0   |        the wallet don't be encrypt        |
|   1   | the wallet be encrypt but don't be locked |
|   2   |     the wallet be encrypt and locked      |

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"islocked"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "islock":0
  },
  "error": null,
  "id": "curltext"
}
```


---
