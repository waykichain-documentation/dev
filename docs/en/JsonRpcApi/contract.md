<extoc></extoc>

# Smart Contract API 

---


## registercontracttx
create a register script transaction

**smart contract can be published by any address, and the publisher is not necessarily the owner of the contract.**

**The smart contract file want to be published must be in directory of `/tmp/lua/`**

**Parameters**

`address` smart contract publisher's address

`filepath` the path of smart contract want to be published

`fee` cost of publishing smart contract（110000000 `sawi` etimated）

**Returns**

`hash` transaction hash of publishing smart contract

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
Get an object containing regid

**Parameters**

`txhash` the transaction hash

**Returns**

`regid`    smart contract regid

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractregid","params":["641794c5db4af756660197878dce462b887224a2efad8387d915bbc2acb5aa9d"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "regid:":"314219-2",
    },
    "error":null,
    "id":"curltext"
}
```


---


## getcontractaccountinfo
get appaccount info

**Parameters**

`regid` regid of smart contract

`address`  the address/regid

**Returns**

`mAccUserID` The address is converted to a string after hexadecimal

`FreeValues` the value of free coins for smart contract

`vFreezedFund` frozen coins and unfreeze time for smart contract

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
call a smart contract function. The smart contract must have been submitted

**Parameters**

`userregid` the smart contract caller address/regid

`regid`    smart contract regid

`amount`   transfer WICC to smart contract,unit is `sawi`

`arguments`  call the parameters of the smart contract

`fee`, cost of call smart contract, smallest fee **100000 sawi**

**Returns**

`hash` the transaction hash

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
get the app data by given regid.

**Parameters**

`regid` smart contract regid

`key`   the key of data in smart contract


**Returns**

`regid`  smart contract regid

`key`  the key of data in smart contract

`value` the value of data in smart contract

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
get the app data by given regid.

**Parameters**

`regid` smart contract regid

`key` the key of data in smart contract,Character ASCII code to hexadecimal


**Returns**

`regid` smart contract regid

`key` the key of data in smart contract,Character ASCII code to hexadecimal

`value` the value of data in smart contract,Character ASCII code to hexadecimal

**Example**

```json
// Request  Character ASCII code "admin" to hexadecimal ：“61646d696e”
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
get the list register script.

**Parameters**

`showDetail` show script content or not. `true`:showDetail, otherwise not show

**Returns**

`scriptId` smart contract regid

`description`smart contract description

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
get smart contract information

**Parameters**

`regid` smart contract regid

**Returns**

`scriptId` smart contract regid

`description` smart contract description

`scriptContent` smart contract content

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
