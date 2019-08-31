<extoc></extoc>

# 钱包相关 API
---

## encryptwallet
对钱包进行设置密码，此时钱包并未加密

**Parameters**

`passphrase` 本钱包加密的密码

**Returns**

`encrypt` - `true`:加密成功`false`:加密失败

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
对解锁钱包进行解锁，手动设置解锁持续的时间，单位为秒

**Parameters**

`passphrase` 本钱包加密的密码

`timeout` 开启解锁状态持续的时间,单位为秒

**Returns**

`passphrase` - `true`:解锁成功、`false`:解锁失败

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
对钱包进行锁定, 解锁请调用 `walletpassphrase` 方法

**Parameters**

none

**Returns**

`walletlock` - `true`:锁定成功`false`:锁定失败

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
修改钱包的加密密码

**Parameters**

`oldpassphrase` 旧密码

`newpassphrase` 新密码

**Returns**

`chgpwd` 密码是否修改成功

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
导出钱包

**Parameters**

`filename` 导出的钱包路径+命名

**Returns**

`info` 导出钱包的结果信息

`key size` 导出钱包中包含的地址账号数量

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
导入钱包


**Parameters**

`filename` 导入的钱包路径+名称，钱包是由 [dumpwallet](#dumpwallet) 导出

**Returns**

`imorpt key size` 导入钱包中包含的账号数量

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
备份钱包,即备份`wallet.dat`文件到本地


**Parameters**

`destination` 备份的钱包路径+名称

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
获取钱包信息

**Parameters**

none

**Returns**

`wallet_version` 钱包版本

`wallet_balance` 钱包余额

`wallet_encrypted` (boolean) 钱包是否已加密

`unlocked_until` (boolean) 钱包是否是锁住状态

`coinfirmed_tx_num` 钱包里已确认的交易数量

`unconfirmed_tx_num` 钱包里未确认的交易数量


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
