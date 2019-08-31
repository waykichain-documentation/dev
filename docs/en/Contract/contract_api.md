# The smart contract provide  interface for interaction between contract and node
All interface functions are in lib which will be named **mylib**, Developers need to use **Lua** to develop

<extoc></extoc>

---

## WriteData
Write data to blockchain DB

**Prototype of function**

`mylib.WriteData(writeDbTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  writeDbTbl  | table |

**example**
```lua
local writeDbTbl = {
    key = "config",
    length = 0,
    value = {}
}

```


**Returns**



**Example**
```lua
function mylib_WriteData()
    local writeDbTbl = {
        key = "config",
        length = 0,
        value = {}
    }

    addressTbl = {0x64,0x63,0x6D,0x43,0x62,0x4B,0x62,0x41,
    0x66,0x4B,0x72,0x6F,0x66,0x4E,0x7A,0x33,
    0x35,0x4D,0x53,0x46,0x75,0x70,0x78,0x72,
    0x78,0x34,0x55,0x77,0x6E,0x33,0x76,0x67,
    0x6A,0x4C}

    writeDbTbl.value = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x64,
    0x00,0x00,0x00,0x00,0x00,0x00,0x01,0x2C,
    Unpack(addressTbl)}
    writeDbTbl.length = #writeDbTbl.value
    assert(mylib.WriteData(writeDbTbl),"WriteData err")
end
```


---

## ReadData
read the data which in blockchain DB by key

**Prototype of function**

`mylib.ReadData(key)`

**Parameters**

| parameter |  type  |
|:---------:|:------:|
|    key    | String |


**Returns**



**Example**
```lua
function mylib_ReadData()
    local key ="config"
    local readResult = {mylib.ReadData(key)}
    assert(#readResult > 0,"ReadData0 err")
    local i
    for i = 1,#readResult do
        print("",i,(readResult[i]))
    end
end
```

---

## ModifyData
modify the data which in blockchain DB by key

**Prototype of function**

`mylib.ModifyData(writeDbTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  writeDbTbl  | table |

**example**
```lua
local writeDbTbl = {
    key = "config",
    length = 0,
    value = {}
}
```


**Returns**



**Example**
```lua
function mylib_ModifyData()
    local writeDbTbl = {
        key = "config",
        length = 0,
        value = {}
    }
    addressTbl = {0x64,0x63,0x6D,0x43,0x62,0x4B,0x62,0x41,
    0x66,0x4B,0x72,0x6F,0x66,0x4E,0x7A,0x33,
    0x35,0x4D,0x53,0x46,0x75,0x70,0x78,0x72,
    0x78,0x34,0x55,0x77,0x6E,0x33,0x76,0x67,
    0x6A,0x4C}


    writeDbTbl.value = {0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x64,
    0x00,0x00,0x00,0x00,0x00,0x00,0x01,0x2C,
    Unpack(addressTbl)}
    writeDbTbl.length = #writeDbTbl.value
    writeDbTbl.value[8] = 200
    assert(mylib.ModifyData(writeDbTbl),"ModifyData err")
    print("ModifyData ok")
    readResult = {}
    readResult = {mylib.ReadData(writeDbTbl.key)}
    assert(#readResult > 0,"ReadData1 err")
    print("ReadData1 ok")
    for i = 1,#readResult do
        print("",i,(readResult[i]))
    end
end
```

---

## LogPrint
Print log

**Prototype of function**

`mylib.LogPrint(LogTable)`

**Parameters**

| parameter | type |
|:--------- |:---- |
|   log        | table     |

**example**
```
local LogTable = {

key = 0 , --log type:LOG_TYPE.ENUM_STRING/LOG_TYPE.ENUM_NUMBER

length = 0, --len of value

value = nil -- string or bytes

}
```

**Returns**

* none

**Example**
```lua
function mylib_LogPrint()
    local LogTable = {
        key = LOG_TYPE.ENUM_STRING,
        length = 0,
        value = nil
    }

    LogTable.length = 9
    LogTable.value = "lua start"
    mylib.LogPrint(LogTable)
    LogTable.key = LOG_TYPE.ENUM_NUMBER
    LogTable.length = 20
    local i
    for i = 1,20 do
        LogTable.value [i] = i
    end
    mylib.LogPrint(LogTable)
end
```



---

## WriteOutput
Operating system account, be used to transfer WIIC to dst address.

**Prototype of function**

`mylib.WriteOutput(writeOutputTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  writeOutputTbl  | table |


**Returns**

success：True；

failed：False

**Example**

Refer to [TransferToAddr][1]


---
## GetScriptID
Get regid of smart contract

**Prototype of function**

`mylib.GetScriptID()`

**Parameters**

* none

**Returns**

success : 6 bytes regid

failed : nil

**Example**
```lua
function mylib_GetScriptID()
    local result = {mylib.GetScriptID())}
    assert(#result > 0," GetScriptID err")
end
```

---
## GetCurTxAccount
Get current tx account

**Prototype of function**

`mylib.GetCurTxAccount()`

**Parameters**

* none

**Returns**



**Example**
```lua
function mylib_GetCurTxAccount()
    local i
    local result = {mylib.GetCurTxAccount())}
    assert(#result == 6," GetCurTxAccount err");
    for i = 1,#result do
        print("Account",i,(result[i]))
    end
end
```

---
## GetCurTxPayAmount
Get current Tx PayAmount

**Prototype of function**

`mylib.GetCurTxPayAmount()`

**Parameters**

* none

**Returns**

success：8 bytes amount, uint is `sawi`, use `mylib.ByteToInteger` to convert to data of type Number

failed：nil

**Example**
```lua
function mylib_GetCurTxPayAmount()
    local i
    local paymoneyTbl = {mylib.GetCurTxPayAmount())}
    assert(#paymoneyTbl == 8," GetCurTxPayAmount err");
    for i = 1,# paymoneyTbl do
        print("PayAmount ",i,( paymoneyTbl [i]))
    end
end
```

---

## GetUserAppAccValue
Get the token balance for the specified account

**Prototype of function**

`mylib.GetUserAppAccValue(idTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  34 byes of base58 address | table |

**example**
```lua
-- 用户id
local idTbl = {
    idLen = 0, -- len of base58 address
    idValueTbl = {} --base58 address
}
```

**Returns**

success：8 bytes balance, uint is `sawi`, use `mylib.ByteToInteger` to convert to data of type Number

failed：nil

**Example**
```lua
function mylib_GetUserAppAccValue()
    local idTbl = {
        idLen = 34, --base58地址长度
        idValueTbl = {0x77,0x68,0x4d,0x31,0x64,0x4c,0x6d,0x59,0x62,0x38,0x75,0x41,0x50,0x45,0x61,0x65,0x43,0x4d,0x46,0x4b,0x72,0x74,0x37,0x46,0x4a,0x57,0x46,0x4d,0x64,0x53,0x32,0x6a,0x4b,0x67} --base58地址
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

## WriteOutAppOperate
Operating system application account. Used for token issuance, transfer, freezing, etc.

**Prototype of function**

`mylib.WriteOutAppOperate(app_operateTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  app\_operateTbl  | table |

**example**
```lua
local app_operateTbl = {
    operatorType = 0, --type of operate
    outHeight = 0, --height of timeout
    moneyTbl = {}, --amount
    userIdLen = 0, --len of regid
    userIdTbl = {}, --regid of address
    fundTagLen = 0, --fund tag len
    fundTagTbl = {} --fund tag
}
```

**Returns**

success：True；

failed：false

**Example**

Refer to [TransferToken][2]


---

## GetBase58Addr
Get address with a coding of Base58

**Prototype of function**

`mylib.GetBase58Addr(Unpack(accountTbl))`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  accountTbl  | table |

**example**
```
--6 bytes regid
local accountTbl = {5,157,0,0,7,34}
```


**Returns**

success：34 bytes base58 address；

failed：nil

**Example**
```lua
function mylib_GetBase58Addr()
    local accountTbl = {5,157,0,0,7,34} --6 bytes regid
    local result = {mylib.GetBase58Addr(Unpack(accountTbl))}
    assert(#result == 34,"GetBase58Addr err")
end
```

---

## ByteToInteger
Convert bytes to Number

**Prototype of function**

`mylib.ByteToInteger(Unpack(byteTbl))`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  4/8 bytes data  | table |

**Returns**

success：Number data

failed：nil


**Example**
```lua
function mylib_ByteToInteger()
    local height = mylib.ByteToInteger(0xa0,0x05,0x00,0x00)
    assert(height > 0,"ByteToInteger error0")
    print("height=",height)
    local money = mylib.ByteToInteger(0x78,0x56,0x34,0x12,0x78,0x56,0x34,0x12)
    assert(money > 0,"ByteToInteger error1")
    print("money=",money)
end
```

---

## IntegerToByte8
Convert Number to byte8

**Prototype of function**

`mylib.ModifyData(writeDbTbl)`

**Parameters**

| parameter | type  |
|:---------:|:-----:|
|  number  | Number |

**Returns**

success：8 bytes data

failed：nil

**Example**
```lua
function mylib_IntegerToByte8()
    local money = 1311768465173141112
    local result = {mylib.IntegerToByte8(money)}
    assert(#result == 8,"IntegerToByte8 error0")
    for i = 1,#result do
        print("",i,(result[i]))
    end
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
        LogMsg(LOG_TYPE.ENUM_STRING,"VerifySignature ok!")
    else
        LogMsg(LOG_TYPE.ENUM_STRING,"VerifySignature bad!")
    end
end
```

---
## QueryAccountBalance
Get balance of WICC from dst address

**Prototype of function**

`mylib.QueryAccountBalance(Unpack(accountTbl))`

**Parameters**

accountTbl: 34 bytes address

**Returns**

success：8 bytes balance, uint is `sawi`, use `mylib.ByteToInteger` to convert to data of type Number

failed：nil

**Example**
```lua
function mylib_QueryAccountBalance()
    local accountTbl = {0x77,0x4c,0x4b,0x66,0x32,0x4e,0x71,0x77,0x74,0x48,0x6b,0x33,0x42,0x66,0x7a,0x4b,0x35,0x77,
        0x4d,0x44,0x66,0x62,0x4b,0x59,0x4e,0x31,0x53,0x43,0x33,0x77,0x65,0x79,0x52,0x34}

    local result = {mylib.QueryAccountBalance(Unpack(accountTbl))}
    assert(#result == 8,"QueryAccountBalance err");
end
```

---
## GetCurTxHash
Get the current transaction hash

**Prototype of function**

`mylib.GetCurTxHash()`

**Parameters**

* none

**Returns**

|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| hashTbl |  table  |  {0x46,0x78,0xd3,0xc5,0x4c,0xaf,0x3b,0xd7,0xaf,0x59,0x1d,<br>0xb4,0x85,0x21,0xf9,0xb1,0xf6,0x98,0x12,0x90,0x9c,0x56,0x19,<br>0xba,0x7a,0x46,0x22,0x6b,0xb7,0x2b,0x61,0xd0}     |  success    |
|    nil       |       |       |   failed   |

**Example**
```lua
function mylib_GetCurTxHash()
    local result = {mylib.GetCurTxHash()}
    assert(#result == 32,"GetCurTxHash err");
end
```

---
## GetTxConFirmHeight
Get the transaction confirmation height based on the transaction hash

**Prototype of function**

`mylib.GetTxConfirmHeight(Unpack(txhashTbl))`

**Parameters**

|  parameter   | type  |
|:-------:|:-----:|
| transaction hash | table |


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

## GetBlockHash
Get the block hash based on the block height

**Prototype of function**

`mylib.GetBlockHash(height)`

**Parameters**

|  parameter   | type  |
|:-------:|:-----:|
| block height | Number |


**Returns**

|return | type | example |destination|
| :----     | :---- | :---- | :--- |
| height |  table  |  {0x46,0x78,0xd3,0xc5,0x4c,0xaf,0x3b,0xd7,0xaf,<br>0x59,0x1d,0xb4,0x85,0x21,0xf9,0xb1,0xf6,<br>0x98,0x12,0x90,0x9c,0x56,0x19,0xba,0x7a,<br>0x46,0x22,0x6b,0xb7,0x2b,0x61,0xd0}   |  success    |
|    nil       |       |       |   failed   |

**Example**
```lua
function mylib_GetBlockHash()
    local height = 47037
    local result = {mylib.GetBlockHash(height)}
    assert(#result == 32,"GetBlockHash err");
end
```

---
## GetBlockTimestamp
Get block Timestamp

**Prototype of function**

`mylib.GetBlockTimestamp(height)`

**Parameters**

`height` if \< =0 ,height = lastheight - |height|

`height` if \> 0  height = height

**Returns**

success：unix timestamp

failed：nil


**Example**
```lua
function mylib_GetBlockTimestamp()
    local ts = mylib.GetBlockTimestamp(0)
    assert(ts ~= nil,"ts is nil")
    str = string.format("0:%d", ts)
    LogMsg(LOG_TYPE.ENUM_STRING,string.len(str),str)
    ts = mylib.GetBlockTimestamp(100)
    assert(ts ~= nil,"ts is nil")
    str = string.format("100:%d", ts)
    LogMsg(LOG_TYPE.ENUM_STRING,string.len(str),str)
    ts = mylib.GetBlockTimestamp(-100)
    assert(ts ~= nil,"ts is nil")
    str = string.format("-100:%d", ts)
    LogMsg(LOG_TYPE.ENUM_STRING,string.len(str),str)
end
```

---


[1]:	./api_debug.md/#transfertoaddr
[2]:	./api_debug.md/#transfertoken