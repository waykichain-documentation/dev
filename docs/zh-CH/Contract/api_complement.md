
# Smart contract complement

<extoc></extoc>

---

## GetTxRegID
获取指定hash的交易发起账户

**Prototype of function**

`mylib.GetTxRegID(hashTbl)`

**Parameters**

|  parameter   | type  |
|:-------:|:-----:|
| 交易哈希 | table |


**Returns**

成功：返回6字节的账户ID

失败：nil

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
获取账户的公钥

**Prototype of function**

`mylib.GetAccountPublickey(Unpack(accountTbl))`

**Parameters**

6字节的账户ID

**Returns**


|return | type | example |destination|
| :----     | :---- | :----:                                                                                                                                                                  | :----:              |
| publickey | table | {0x03,0x6c,0x53,0x97,0xf3,0x22,<br>0x7a,0x1e,0x20,0x99,0x52,0x82,0x9d,<br>0x24,0x9b, 0x7a,0xd0,0xf6,0x15,0xe4,0x3b,<br>0x76,0x3a,0xc1,0x5e,0x3a,0x6f,<br>0x52,0x62,0x7a,0x10,0xdf,0x21} | success (33 bytes) |
| nil       |       |                                                                                                                                                                        | failed             |


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

## GetBlockHash
根据区块高度获取区块哈希

**Prototype of function**

`mylib.GetBlockHash(height)`

**Parameters**

|  parameter   | type  |
|:-------:|:-----:|
| 区块高度 | Number |


**Returns**

|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| height |  table  |  {0x46,0x78,0xd3,0xc5,0x4c,0xaf,0x3b,0xd7,0xaf,<br>0x59,0x1d,0xb4,0x85,0x21,0xf9,0xb1,0xf6,<br>0x98,0x12,0x90,0x9c,0x56,0x19,0xba,0x7a,<br>0x46,0x22,0x6b,0xb7,0x2b,0x61,0xd0}   |  success    |
|    nil       |       |       |   failed   |

**Example**
```lua
function mylib_GetBlockHash()
    local height = 47037
    local i
    local result = {mylib.GetBlockHash(height)}
    assert(#result == 32,"GetBlockHash err");
    for i = 1,#result do
        print("BlockHash",i,(result[i]))
    end
end
```

---

## GetCurRunEnvHeight
获取当前运行高度

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
删除脚本数据库

**Prototype of function**

`mylib.DeleteData(key)`

**Parameters**

| parameter |  type  |
|:---------:|:------:|
|    key    | String |


**Returns**

成功：true；失败：false

**Example**
```lua
function mylib_DeleteData()
    local writeDbTbl = {
        key = "config", --关键字
        length = 0, -- value数据流的总长
        value = {} --值
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
获取智能合约脚本key-value数据

**Prototype of function**

`mylib.GetScriptData(paraTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  paraTbl  | table |

**example**
```lua
local paraTbl = {
    id = {0x00,0x01,0x00,0x00,0xb7,0xc4}, --6字节的智能合约脚本id
    key = "config", --关键字
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
        --gid = {0x00,0x01,0x00,0x00,0xb7,0xc4}, --6字节的脚本id
        id = {mylib.GetScriptID()},
        key = "config", --关键字
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
获取指定账户的余额

**Prototype of function**

`mylib.GetUserAppAccFoudWithTag(app_operateTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  用户ID  | table |

**example**
```lua
-- 用户id
local idTbl = {
    idLen = 0, --id长度
    idValueTbl = {} --id值
}
```

**Returns**

成功：返回Int64的交易金额；失败：nil

**Example**
```lua
function mylib_GetUserAppAccValue()
    local idTbl = {
        idLen = 6, --id长度
        idValueTbl = {0x01,0x02,0x03,0x04,0x05,0x06} --id值
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
将 Number类型值转换成4字节类型

**Prototype of function**

`mylib.IntegerToByte4(height)`

**Parameters**

| parameter | type  | Note|
|:---------:|:-----:| :----|
|  Interger  | Number | 0 ~ 2^32-1|

**Returns**

成功：返回4个字节的字节流
失败：nil

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
Sha256 hash加密算法

**Prototype of function**

`mylib.Sha256(string))`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  待加密的内容  | all |


**Returns**

加密输出，成功返回一个table，否则nil

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
Des加密算法

**Prototype of function**

`mylib.Des(desTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  待加密的内容  | table |

**example**
```lua
local desTbl =
{
    dataLen = 0, --加密数据长度
    data = {}, --加密数据
    keyLen = 0, --私钥长度
    key = {}, --私钥数据
    flag = 1 --1加密 0解密
}
```

**Returns**

加密输出，成功返回一个table，否则nil

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
验证签名

**Prototype of function**

`mylib.VerifySignature(sigTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  待验证的内容  | table |

**example**
```lua
local sigTbl =
{
    dataLen =0, --原始数据长度
    data = {}, --原始数据
    pubKeyLen = 0, --签名公钥长度
    pubKey = {}, --签名公钥
    signatureLen = 0, --签名长度
    signature = {} --签名
}
```

**Returns**

true：验证成功，false：失败

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
获取指定hash的合约内容

**Prototype of function**

`mylib.GetTxContracts(hashTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  合约交易hash  | table |

**Returns**

成功：返回合约内容；失败：nil

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
转移全部资产

**Prototype of function**

`mylib.TransferContactAsset(Unpack(addrTbl))`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  账户地址  | table |

**Returns**

成功：True；失败：False

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
转移部分资产

**Prototype of function**

`mylib.TransferSomeAsset(assetOperateTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  assetOperateTbl  | table |

**example**
```lua
local assetOperateTbl = {
    toAddrTbl = {}, --转移目的地址
    outHeight = 0, --高度
    moneyTbl = {}, --资产数
    fundTagLen = 0, --fund tag len
    fundTagTbl = {} --fund tag
}
```

**Returns**

成功：True；失败：False

**Example**
```lua
function mylib_TransferSomeAsset()
    local accountTbl = {5,157,0,0,7,34} --6字节的账户ID
    local addrTbl = {mylib.GetBase58Addr(Unpack(accountTbl))}
    assert(#addrTbl > 0,"GetBase58Addr err")
    local assetOperateTbl = {
        toAddrTbl = {}, --转移地址
        outHeight = 0, --高度
        moneyTbl = {}, --资产数
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

