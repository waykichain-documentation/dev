<extoc></extoc>

# Wallet API  
---

## encryptwallet
Encrypts the wallet with passphrase, instead of lock wallet

**Parameters**

`passphrase` password

**Returns**

`encrypt` - `true`:encrypt successful`，false`:encrypt failed

**Example**

```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"encryptwallet","params":["12345678"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "encrypt":true
  },
  "error": null,
  "id": "curltext"
}
```


---


## walletpassphrase
Unlock wallet for<timeout> seconds.

**Parameters**

`passphrase` password

`timeout` The time in which the state is unlocked, unit is second

**Returns**

`passphrase` - `true`:unlocked successful`，false`:unlocked failed

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"walletpassphrase","params":["12345678",1000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "passphrase":true
  },
  "error":null,
  "id":"curltext"
}
```


---


## walletlock
locking the wallet. After calling this method, you will need to call walletpassphrase again before being able to call any methods which require the wallet to be unlocked.

**Parameters**

none

**Returns**

`walletlock` - `true`:locked successful`，false`:locked failed

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"walletlock","params":[]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "walletlock":true
  },
  "error": null,
  "id": "curltext"
}

```

---


## walletpassphrasechange
Changes the wallet passphrase from <oldpassphrase> to <newpassphrase>.

**Parameters**

`oldpassphrase` old passphrase

`newpassphrase` new passphrase

**Returns**

`chgpwd` - `true`:changed successful`，false`:changed failed

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"walletpassphrasechange","params":["123456789","1234567890"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "chgpwd":true
  },
  "error": null,
  "id": "curltext"
}
```

---


## dumpwallet
Dump all wallet keys in a human-readable format dnd write to <filename>

**Parameters**

`filename` the path+filename to dump wallet

**Returns**

`info` result

`key size` the size of address  in dumpwallet

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"dumpwallet","params":["/opt/wicc/walletfilename"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "info":"dump ok",
    "key size":14
  },
  "error": null,
  "id": "curltext"
}

```

---


## importwallet
Import a wallet

**Parameters**

`filename` the path+filename to import wallet, dumpwallet from [dumpwallet][1]

**Returns**

`imorpt key size` the size of address  in import wallet

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"importwallet","params":["/opt/wicc/walletfilename"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
  "result":
  {
    "imorpt key size":14
  },
  "error": null,
  "id": "curltext"
}
```
---

## backupwallet
backup wallet file `wallet.dat` to local


**Parameters**

`destination` the path + name to backup wallet 

**Returns**


**Example**
```json
// Request
curl -u waykichain:wicc123 -d '{"jsonrpc":"2.0","id":"curltext","method":"backupwallet","params":["/opt/wicc/walletfilename"]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result": null,
    "error": null,
    "id": "curltext"
}

```
---
## getwalletinfo
Get an object containing various wallet state info

**Parameters**

none

**Returns**

`wallet_version` the wallet version

`wallet_balance` the total Coin balance of the wallet

`wallet_encrypted` (boolean) whether the wallet is encrypted or not

`unlocked_until` (boolean) whether the wallet is locked or not

`coinfirmed_tx_num` the size of transactions in the wallet

`unconfirmed_tx_num` the size of unconfirmtx transactions in the wallet

**Example**
```json
// Request
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getwalletinfo"}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result": {
        "wallet_version": 0,
        "wallet_balance": 207899994.89999995,
        "wallet_encrypted": false,
        "wallet_locked": false,
        "unlocked_until": 0,
        "coinfirmed_tx_num": 3189,
        "unconfirmed_tx_num": 0
    },
    "error": null,
    "id": "curltext"
}
```

---

[1]:	#dumpwallet
