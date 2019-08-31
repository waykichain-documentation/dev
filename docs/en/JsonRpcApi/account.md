<extoc></extoc>

# API about account
---


## listaddr
Get info about addresses in current node

**Parameters**

none

**Returns**

`addr` address

`balance` balance

`haveminerkey` minerkey (ignore)

`regid` register ID

**Example**

// Request
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"listaddr","params":[]}' -H 'content-type:application/json;' http://127.0.0.1:6967
```

// Response
```json
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


## getnewaddr
Create a new address

**Parameters**

none

**Returns**

`addr` address

`minerpubkey` miner pubkey,no miner pubkey - no

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
Register an account. The newly created address must be registered to initiate a transference.
**NOTE**：`registeraccounttx`、`registaccounttx` backward compatible

**Requires unlocked wallet**

Y

**Parameters**
`address` The address you want to be registered

`fee` cost of register **The unit is `sawi`, smallest fee **10000 sawi\*\*

**Returns**

`hash` transaction hash

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
 Used to create a register account transaction signature data ,which can be broadcasted on the blockchain using the [submittx][1] method


**Parameters**

`fee` cost of register **The unit is `sawi`**

`height` block height at register

`publickey` the public key

**Returns**

`rawtx` signature data

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
Get general account or contract account address details

**Parameters**

`address`/`regid` common account/contract account address/regid

**Returns**

`Address` Ordinary account/contract account address

`KeyID`   sha256((public key))

`RegID`   registered id

`PublicKey` public key(Ordinary account only）

`MinerPKey` Miner Key (ignored)

`Balance` Balance **The unit is `sawi`**

`Votes` Votes（use for Dapp vote）

`UpdateHeight` (ignored)

`voteFundList` vote list

`position` Did not receive the coin：`inwallet` ，received the coin：`inblock`

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
Reveals the private key by address

**Parameters**

`address` Common account address in this wallet

**Returns**

`privatekey` private key（WIF format）

`minerkey`   miner identification (ignored)

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
import privkey（from method `dumpprivkey`）to wallet

**Requirement**

the wallet must be unlocked.

**Parameters**

`privkey` common account private key（WIF format）

**Returns**

`imorpt key address` common account address

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
Check common account or contract account address is valid or not

**Parameters**

`address` common account/contract account address/regid

**Returns**

`ret` = `true` valid

`ret` = `true` invalid

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

[1]:	./tx.md/#submittx
