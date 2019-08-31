
# Smart contract Usage

<extoc></extoc>

---

## GetTxRegID
Get the calling account address by hash.

**Prototype of function**

`mylib.GetTxRegID(hashTbl)`

**Parameters**

|  parameter   | type  |
|:-------:|:-----:|
| tx hash | table |

**Returns**

success：6 bytes regid of address

failed：nil

**Example**
```lua
function mylib_GetTxRegID()
    local hash = {0x24, 0x4f, 0xa7, 0xcf, 0x97, 0xae, 0x15, 0x85, 0xd8, 0xf8, 0x02, 0x4b, 0xa1, 0x8b, 0x8a, 0xbe, 0xce, 0x8e, 0xb9, 0xcd, 0x4d, 0x01, 0x6d, 0xd0, 0xba, 0x8c, 0xc0, 0xdc, 0x85, 0x1a, 0x9c, 0x0e}
    local accounts = {mylib.GetTxRegID(Unpack(hash))}
    LogMsg(LOG_TYPE.ENUM_NUMBER,# accounts, accounts)
end
```

---

## GetAccountPublickey
Get the Publickey of address

**Prototype of function**

`mylib.GetAccountPublickey(Unpack(accountTbl))`

**Parameters**

* none

**Returns**


|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| publickey |  table  |{0x03,0x6c,0x53,0x97,0xf3,0x22,0x7a,0x1e,0x20,<br>0x99,0x52,0x82,0x9d,0x24,0x9b,0x7a,0xd0,<br>0xf6,0x15,0xe4,0x3b,0x76,0x3a,0xc1,0x5e,<br>0x3a,0x6f,0x52,0x62,0x7a,0x10,0xdf,0x21}       |  success (33 bytes)   |
|    nil       |       |       |   failed   |


**Example**
```lua
function mylib_GetAccountPublickey()
    local accountTbl = {mylib.GetCurTxAccount()}
    local i
    local result = {mylib.GetAccountPublickey(Unpack(accountTbl))}
    assert(#result == 33,"GetAccountPublickey err");
    for i = 1,#result do
        print("Publickey",i,(result[i]))
    end
end
```


---
## GetTxConfirmHeight
Get Tx confirmed height

**Prototype of function**

`mylib.GetTxConfirmHeight(Unpack(txhashTbl))`

**Parameters**

|  parameter   | type  |
|:-------:|:-----:|
| tx hash | table |


**Returns**

|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| height |  Number  |  1234.0    |  success    |
|    nil       |       |       |   failed   |


**Example**
```lua
function mylib_GetTxConfirmHeight()
    -- "hash" :"4a2af2d83683325e780f2b859e7421f4592e3105d01017aab45c15da3910be8e"
    local txhashTbl = {0x4a,0x2a,0xf2,0xd8,0x36,0x83,0x32,0x5e,
    0x78,0x0f,0x2b,0x85,0x9e,0x74,0x21,0xf4,
    0x59,0x2e,0x31,0x05,0xd0,0x10,0x17,0xaa,
    0xb4,0x5c,0x15,0xda,0x39,0x10,0xbe,0x8e}
    local result = mylib.GetTxConfirmHeight(Unpack(txhashTbl))
    assert(result > 0,"GetTxConfirmHeight err");
end
```

---

## GetCurRunEnvHeight
Get current running height

**Prototype of function**

`mylib.GetCurRunEnvHeight()`

**Parameters**

* none

**Returns**

|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| height |  Number  |  1234    |  success    |
|    nil       |       |       |   failed   |

**Example**
```lua
function mylib_GetCurRunEnvHeight()
    local result = mylib.GetCurRunEnvHeight()
    assert(result > 0,"GetCurRunEnvHeight err");
    print("RunEnvHeight",result)
end
```

---

## DeleteData
delete the data in blockchain DB

**Prototype of function**

`mylib.DeleteData(key)`

**Parameters**

| parameter |  type  |
|:---------:|:------:|
|    key    | String |


**Returns**



**Example**
```lua
function mylib_DeleteData()
    local writeDbTbl = {
        key = "config",
        length = 0,
        value = {}
    }
    assert(mylib.DeleteData(writeDbTbl.key),"DeleteData err")
    print("DeleteData return ok")
    local readResult = {}
    readResult = {mylib.ReadData(writeDbTbl.key)}
    if(TableIsEmpty(readResult)) then
        print("DeleteData ok")
    end
end
```



---
## GetScriptData
Get key-values data in smart contract.

**Prototype of function**

`mylib.GetScriptData(paraTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  paraTbl  | table |

**example**
```lua
local paraTbl = {
    id = {0x00,0x01,0x00,0x00,0xb7,0xc4},  --6 bytes regid of smart contract
    key = "config",   -- key
}
```

**Returns**

|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| result |  table  |  {0x31,0x32}    |  success    |
|    nil       |       |       |   failed   |

**Example**
```lua
function mylib_GetScriptData()

    mylib_WriteData()
    local paraTbl = {
        --id = {0x00,0x01,0x00,0x00,0xb7,0xc4},
        id = {mylib.GetScriptID()},
        key = "config",
    }
    local result = {mylib.GetScriptData(paraTbl)}
    assert(#result > 0,"GetScriptData err")
    for i = 1,#result do
        print("GetScriptData",i,(result[i]))
    end
end
```

---
## GetUserAppAccFoudWithTag
Get balance using account account

**Prototype of function**

`mylib.GetUserAppAccFoudWithTag(app_operateTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  user ID  | table |

**example**
```lua
-- userid
local idTbl = {
    idLen = 0, --id length
    idValueTbl = {} --id
}
```

**Returns**


**Example**
```lua
function mylib_GetUserAppAccValue()
    local idTbl = {
        idLen = 6, --id length
        idValueTbl = {0x01,0x02,0x03,0x04,0x05,0x06} --id
    }
    local i
    local money = { mylib.GetUserAppAccValue(idTbl) }
    assert(#money == 8," GetUserAppAccValue err");
    for i = 1,# money do
        print("money ",i,( money [i]))
    end
end
```

---

## IntegerToByte4
Convert Number to byte4

**Prototype of function**

`mylib.IntegerToByte4(height)`

**Parameters**

| parameter | type  | Note|
|:---------:|:-----:| :----|
|  Interger  | Number | 0 \~ 2^32-1|

**Returns**

success：4 bytes data

failed：nil

**Example**
```lua
function mylib_IntegerToByte()
    local height = 1440
    local result = {mylib.IntegerToByte4(height)}
    assert(#result == 4,"IntegerToByte4 error0")
    local i
    print("height byte")
    for i = 1,#result do
        print("",i,(result[i]))
    end
end
```

---

## Sha256
Sha256 Hash algorithm

**Prototype of function**

`mylib.Sha256(string))`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  unencrypted data  | all |


**Returns**



**Example**
```lua
function mylib_Sha256()
    local orgContent = "123"
    local content = {mylib.Sha256(orgContent)}
    LogMsg(LOG_TYPE.ENUM_NUMBER,#content,content)
end
```

---

## Des
Des algorithm

**Prototype of function**

`mylib.Des(desTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  unencrypted data  | table |

**example**
```lua
local desTbl =
{
    dataLen = 0, --date length
    data = {}, --data
    keyLen = 0, --privatekey length
    key = {}, --privatekey
    flag = 1 --1:encrypt 0:decrypt
}
```

**Returns**



**Example**
```lua
function mylib_Des()
    local desTbl =
    {
        dataLen = 8,
        data = {},
        keyLen = 8,
        key = {},
        flag = 1
    }

    desTbl.data = {0xad, 0xdd, 0x1e, 0x1b, 0xeb, 0x8c, 0x10, 0x8d}
    for i = 1,8 do
        desTbl.key[i] = 0x33 + i
    end
    desTbl.flag = 0
    local content = {mylib.Des(desTbl)}
    LogMsg(LOG_TYPE.ENUM_NUMBER,#content,content)
end
```

---

## VerifySignature
verify signature

**Prototype of function**

`mylib.VerifySignature(sigTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|   sigTbl | table |

**example**
```lua
local sigTbl =
{
    dataLen =0, --unsigned date length
    data = {}, --unsigned data
    pubKeyLen = 0, --publickey length
    pubKey = {}, --publickey
    signatureLen = 0, --signature length
    signature = {} --signature
}
```

**Returns**



**Example**
```lua
function mylib_VerifySignature()
    local sigTbl =
    {
        dataLen = 9,
        data = {0x31, 0x32, 0x33, 0x34, 0x35, 0x36, 0x37, 0x38, 0x39},
        pubKeyLen = 33,
        pubKey = {0x03, 0xee, 0xf7, 0xa3, 0x80, 0xbc, 0xf9, 0xcf, 0x97, 0x5d, 0x91, 0x6f, 0xda, 0xb1, 0x8d, 0x08, 0x1c, 0x9d, 0x55, 0xba, 0x43, 0x46, 0x54, 0x35, 0xa4, 0xd1, 0xcc, 0x59, 0x86, 0x10, 0xa4, 0x44, 0x79},
        signatureLen = 32,
        signature = {0x24, 0x4f, 0xa7, 0xcf, 0x97, 0xae, 0x15, 0x85, 0xd8, 0xf8, 0x02, 0x4b, 0xa1, 0x8b, 0x8a, 0xbe, 0xce, 0x8e, 0xb9, 0xcd, 0x4d, 0x01, 0x6d, 0xd0, 0xba, 0x8c, 0xc0, 0xdc, 0x85, 0x1a, 0x9c, 0x0e}
    }

    local ret = mylib.VerifySignature(sigTbl)
    if ret then
        LogMsg(LOG_TYPE.ENUM_STRING,string.len("ok"),"ok")
    else
        LogMsg(LOG_TYPE.ENUM_STRING,string.len("bad"),"bad")
    end
end
```

---

## GetTxContracts
Get  contract data by tx hash

**Prototype of function**

`mylib.GetTxContracts(hashTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  tx hash  | table |

**Returns**


**Example**
```lua
function mylib_GetTxContracts()
    local hash = {0x24, 0x4f, 0xa7, 0xcf, 0x97, 0xae, 0x15, 0x85, 0xd8, 0xf8, 0x02, 0x4b, 0xa1, 0x8b, 0x8a, 0xbe, 0xce, 0x8e, 0xb9, 0xcd, 0x4d, 0x01, 0x6d, 0xd0, 0xba, 0x8c, 0xc0, 0xdc, 0x85, 0x1a, 0x9c, 0x0e}
    local content = {mylib.GetTxContracts(Unpack(hash))}
    LogMsg(LOG_TYPE.ENUM_NUMBER,#content,content)
end
```

---

## TransferContactAsset
Transfer all contact asset

**Prototype of function**

`mylib.TransferContactAsset(Unpack(addrTbl))`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  address  | table |

**Returns**



**Example**
```lua
    function mylib_TransferContactAsset()
    local accountTbl = {5,157,0,0,7,34} --6字节的账户ID
    local addrTbl = {mylib.GetBase58Addr(Unpack(accountTbl))}
    assert(#addrTbl > 0,"GetBase58Addr err")
    assert(mylib.TransferContactAsset(Unpack(addrTbl)), "TransferContactAsset err")
end
```

---

## TransferSomeAsset
transfer some assets

**Prototype of function**

`mylib.TransferSomeAsset(assetOperateTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  assetOperateTbl  | table |

**example**
```lua
local assetOperateTbl = {
    toAddrTbl = {}, --receiver address
    outHeight = 0, --height
    moneyTbl = {}, --amount
    fundTagLen = 0, --fund tag len
    fundTagTbl = {} --fund tag
}
```

**Returns**


**Example**

```lua
function mylib_TransferSomeAsset()
    local accountTbl = {5,157,0,0,7,34} --6字节的账户ID
    local addrTbl = {mylib.GetBase58Addr(Unpack(accountTbl))}
    assert(#addrTbl > 0,"GetBase58Addr err")
    local assetOperateTbl = {
        toAddrTbl = {}, --receiver address
        outHeight = 0, --height
        moneyTbl = {}, --amount
        fundTagLen = 0, --fund tag len
        fundTagTbl = {} --fund tag
    }

    local money = 1311768465173141112
    local moneyTbl = {mylib.IntegerToByte8(money)}
    assetOperateTbl.toAddrTbl = addrTbl
    assetOperateTbl.outHeight = 1440
    assetOperateTbl.moneyTbl = moneyTbl
    assert(mylib.TransferSomeAsset(assetOperateTbl), "TransferSomeAsset err")
end
```

---
