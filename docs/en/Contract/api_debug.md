<extoc></extoc>
# mylib use case

## Local debug library：mylib

**Description**: Since the mylib library is packaged in the chain, the **mylib.lua** for developers is there to debug locally.

**Guide**: The following `mylib.lua` and the development contract file must be in the same directory

mylib.lua:
```lua
mylib = {}

    mylib.constant = "test11"

    function mylib.LogPrint(logTable)
        print("LogPrint" , logTable.key , logTable.value)
    end

    function mylib.GetCurTxAccount()
        return 0x01, 0x02, 0x03, 0x04, 0x05, 0x06
    end

    function mylib.GetBase58Addr(param)
        return 0x77,0x54,0x77,0x72,0x57,0x73,0x65,0x72,0x37,0x38,0x6d,0x45,0x61,0x32,0x32,0x66,0x38,0x6d,0x48,0x66,0x69,0x48,0x47,0x72,0x64,0x4b,0x79,0x73,0x54,0x76,0x38,0x65,0x42,0x55
    end

    function mylib.GetUserAppAccValue(param)
        return 0x00,0x08,0xaf, 0x2f, 0x00, 0x00, 0x00, 0x00
    end

    function mylib.ByteToInteger(param)
        return 10000000000
    end

    function mylib.IntegerToByte8(param)
        return 0x0f
    end

    function mylib.WriteOutAppOperate(param)
        print("param.userIdLen",param.userIdLen)
        return true
    end

    function mylib.WriteOutput(param)
        print("WriteOutput",param)
        return true
    end

    function mylib.GetScriptID()
        return 0x97,0x00,0x00,0x00,0x00,0x00,0x00,0x00
    end

    function mylib.GetCurTxPayAmount()
        return 0x97,0x00,0x00,0x00,0x00,0x00,0x00,0x00
    end

    function mylib.ModifyData(param)
        print("ModifyData: param.value" , param.value)
        return true
    end

    function mylib.WriteData(writeDbTbl)
        print("WriteData:", writeDbTbl.value)
        return true
    end

    function mylib.ReadData(param)
        print("ReadData param" , param)
        if(param=="admin") then
            return 0x77,0x54,0x77,0x72,0x57,0x73,0x65,0x72,0x37,0x38,0x6d,0x45,0x61,0x32,0x32,0x66,0x38,0x6d,0x48,0x66,0x69,0x48,0x47,0x72,0x64,0x4b,0x79,0x73,0x54,0x76,0x38,0x65,0x42,0x55
        elseif(param=="exchangeRate") then
            return 0x00,0xb7,0x07,0x84,0x03,0x00,0x00,0x00
        end
        return 0x97,0x00,0x00,0x00,0x00,0x00,0x00,0x00
    end

return mylib
```

---


## LogMsg
Used to print log information to a file

**Used API**

* [mylib.LogPrint][1]

**Code sample**
```lua
LogMsg = function(LOG_TYPE.ENUM_STRING,Value)
    local LogTable = {
        key = 0, 
        length = 0,
        value = nil 
    }
    
    LogTable.key = LogType
    LogTable.length = #Value
    LogTable.value = Value
    mylib.LogPrint(LogTable)
end
```

---

## GetCurrTxAccountAddress
Get caller address

**Used API**

* [mylib.GetCurTxAccount][2]
* [mylib.GetBase58Addr][3]

**Code sample**
```lua
GetCurrTxAccountAddress = function ()
    return {mylib.GetBase58Addr(mylib.GetCurTxAccount())}
end
```
**Returns**

| type | example                                                                                                                                                                                                      |
|:---------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| table      | {0x77, 0x4c, 0x57, 0x78, 0x43, 0x52, 0x57, 0x44, 0x54, 0x62, 0x33, 0x66, 0x55, 0x54, 0x61, 0x36, 0x7a, 0x74, 0x6f, 0x54, 0x54, 0x43, 0x7a, 0x46, 0x77, 0x44, 0x71, 0x7a, 0x62, 0x63, 0x6b, 0x53, 0x4a, 0x37} |

---

## GetFreeTokenCount
Get the free amount of the current account token

**Used API**

* [mylib.GetUserAppAccValue][4]
* [mylib.ByteToInteger][5]

**Code sample**
```lua
GetFreeTokenCount = function (accountTbl)
    local freeMoneyTbl = { mylib.GetUserAppAccValue( {idLen = #accountTbl, idValueTbl = accountTbl} ) }
    assert(TableIsNotEmpty(freeMoneyTbl), "GetUserAppAccValue error")
    return mylib.ByteToInteger(Unpack(freeMoneyTbl))
end
```

**Returns**

| type   | example |
|:------ |:------- |
| Number |         |

---

## TransferToken
the current account Transfer token to destination address

**Used API**

* [mylib.WriteOutAppOperate][6]
* [mylib.ByteToInteger][7]

**Code sample**
```lua
OP_TYPE = {
    ADD_FREE     = 1,
    SUB_FREE     = 2
}

WriteAppData = function (opType, moneyTbl, userIdTbl)
    local appOperateTbl = {
      operatorType = opType,
      outHeight = 0,
      moneyTbl = moneyTbl,
      userIdLen = #userIdTbl,
      userIdTbl = userIdTbl,
      fundTagLen = 0,
      fundTagTbl = {}
    }
    assert(mylib.WriteOutAppOperate(appOperateTbl), "WriteAppData: ".. opType .." op err")
end

TransferToken = function (fromAddrTbl, toAddrTbl, moneyTbl)
    local money = mylib.ByteToInteger(Unpack(moneyTbl))
    assert(money > 0, money .. " <=0 error")
    local freeMoney = GetFreeTokenCount(fromAddrTbl)
    assert(freeMoney >= money, "Insufficient money to transfer in the account.")
    WriteAppData(OP_TYPE.SUB_FREE, moneyTbl, fromAddrTbl )
    WriteAppData(OP_TYPE.ADD_FREE, moneyTbl, toAddrTbl )
end
```

**Parameters**

|  parameter  | type  | example |
|:-----------:|:-----:| ------- |
| fromAddrTbl | table | {0x77, 0x4c, 0x57, 0x78, 0x43, 0x52, 0x57, 0x44, 0x54, 0x62, 0x33, 0x66, 0x55, 0x54, 0x61, 0x36, 0x7a, 0x74, 0x6f, 0x54, 0x54, 0x43, 0x7a, 0x46, 0x77, 0x44, 0x71, 0x7a, 0x62, 0x63, 0x6b, 0x53, 0x4a, 0x37}        |
|  toAddrTbl  | table | {0x77, 0x4c, 0x44, 0x6e, 0x42, 0x5a, 0x64, 0x36, 0x44, 0x6f, 0x4a, 0x56, 0x54, 0x58, 0x42, 0x4c, 0x44, 0x32, 0x4e, 0x57, 0x51, 0x34, 0x53, 0x45, 0x42, 0x52, 0x68, 0x39, 0x4e, 0x42, 0x76, 0x48, 0x50, 0x71}        |
|  moneyTbl   | table | {0x00 0x50 0xd6 0xdc 0x01 0x00 0x00 0x00}        |

**Returns**

* none

---

## GetCurrTxPayAmount
Get the WICC amount transferred by the contract caller to the smart contract

**Used API**

* [mylib.GetCurTxPayAmount][8]
* [mylib.ByteToInteger][9]

**Code sample**

```lua
GetCurrTxPayAmount = function ()
    local payMoney = mylib.ByteToInteger(mylib.GetCurTxPayAmount())
    assert(payMoney > 0,"GetCurrTxPayAmount: payMoney <= 0")
    return payMoney
end
```

**Returns**

| type   | example |
|:------ |:------- |
| Number | 10 \* 10^8  (unit is `sawi`)|

---

## TransferToAddr
Transfer the WICC in the smart contract to the destination address

**Used API**

* [mylib.WriteOutput][10]
* [mylib.GetScriptID][11]

**Code sample**

```lua
ADDR_TYPE = {
    REGID  = 1,
    BASE58 = 2
}
OP_TYPE = {
    ADD_FREE = 1,
    SUB_FREE = 2
}

WriteAccountData = function (opType, addrType, accountIdTbl, moneyTbl)
  local writeOutputTbl = {
    addrType = addrType,
    accountIdTbl = accountIdTbl,
    operatorType = opType,
    outHeight = 0,
    moneyTbl = moneyTbl
  }
  assert(mylib.WriteOutput(writeOutputTbl),"WriteAccountData" .. opType .. " err")
end

TransferToAddr = function (addrType, accTbl, moneyTbl)
    assert(TableIsNotEmpty(accTbl), "WriteWithdrawal accTbl empty")
    assert(TableIsNotEmpty(moneyTbl), "WriteWithdrawal moneyTbl empty")
    WriteAccountData(OP_TYPE.ADD_FREE, addrType, accTbl, moneyTbl)
    local contractRegid = {mylib.GetScriptID()}
    WriteAccountData(OP_TYPE.SUB_FREE, ADDR_TYPE.REGID, contractRegid, moneyTbl)
    return true
end
```

**Parameters**

| parameter | type      | meaning    | example                                                                                                                                                                                                      |
|:--------- |:--------- |:---------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| addrType  | ADDR\_TYPE | type of address | BASE58                                                                                                                                                                                                       |
| accTbl    | table     | receiver  | {0x77, 0x4c, 0x57, 0x78, 0x43, 0x52, 0x57, 0x44, 0x54, 0x62, 0x33, 0x66, 0x55, 0x54, 0x61, 0x36, 0x7a, 0x74, 0x6f, 0x54, 0x54, 0x43, 0x7a, 0x46, 0x77, 0x44, 0x71, 0x7a, 0x62, 0x63, 0x6b, 0x53, 0x4a, 0x37} |
| moneyTbl  | table     | amount       | {0x00 0x50 0xd6 0xdc 0x01 0x00 0x00 0x00}|

---


## GetContractValue
Get the value of using “key” in the blockchain contract

**Used API**

* [mylib.ReadData][12]

**Code sample**

```lua
GetContractValue = function (key)
    assert(#key > 0, "Key is empty")

    local tValue = { mylib.ReadData(key) }
    if TableIsNotEmpty(tValue) then
      return true,tValue
    else
      LogMsg(LOG_TYPE.ENUM_STRING,"Key not exist.")
      return false,nil
    end
end
```
**Parameters**

|parameter |type |meaning|
| :---- | :---   | :---- |
| key   | String | key   |

**Returns**

| parameter  | type    | meaning      | example |
|:---------- |:------- |:------------ |:------- |
| parameter1 | boolean | exist or not | true    |
| parameter2 | table   | value        | {}      |

---

## WriteOrModify
Write/modify date in the blockChain

**Used API**

* [mylib.WriteData(writeTbl)][13]
* [mylib.ModifyData(writeTbl)][14]

**Code sample**

```lua
WriteOrModify = function (isConfig, writeTbl)
    if not isConfig then
      if not mylib.WriteData(writeTbl) then  error("WriteData error") end
    else
      if not mylib.ModifyData(writeTbl) then error("Modify error") end
    end
end
```

**Parameters**

|parameter |type |meaning|
| :----    | :---   | :---- |
| isConfig | boolean | write or modify   |
| writeTbl        | table       |  data     |

**Returns**

* none

---

## WriteStrkeyValueToDb
Write date into the blockChain

**Code sample**

```lua
WriteStrkeyValueToDb = function (Strkey,ValueTbl)
    local t = type(ValueTbl)
    assert(t == "table","the type of Value isn't table.")

    local writeTbl = {
        key = Strkey,
        length = #ValueTbl,
        value = {}
    }
    writeTbl.value = ValueTbl

    if not mylib.WriteData(writeTbl) then  error("WriteData error") end
end
```
**Parameters**

|parameter |type |meaning|
| :----    | :---   | :---- |
| Strkey | string | key  |
| ValueTbl        | table       |  data     |

**Returns**

* none

[1]:	./contract_api.md/#logprint
[2]:	./contract_api.md/#getcurtxaccount
[3]:	./contract_api.md/#getbase58addr
[4]:	./contract_api.md/#getuserappaccvalue
[5]:	./contract_api.md/#bytetointeger
[6]:	./contract_api.md/#writeoutappoperate
[7]:	./contract_api.md/#bytetointeger
[8]:	./contract_api.md/#getcurtxpayamount
[9]:	./contract_api.md/#bytetointeger
[10]:	./contract_api.md/#writeoutput
[11]:	./contract_api.md/#getscriptid
[12]:	./contract_api.md/#readdata
[13]:	./contract_api.md/#writedata
[14]:	./contract_api.md/#modifydata