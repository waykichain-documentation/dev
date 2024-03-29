
# Smart contract development : WRC20 Sample
<extoc></extoc>

## Source Code
**`WRC20.lua`**
```lua
mylib = require "mylib"

------------------------------------------------------------------------------------

_G.Config={
    -- the standard waykichain contract, if you do not understand the waykichain stardard, please do not change it. Just use the defualt. 
    standard = "WRC20",

    -- the contract owner address, please update it with the actual contract owner address. 
    owner = "wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j",

    -- the contract name, please update it with the actual contract name.
    name = "WRC20N",

    -- the contract symbol, please update it with the actual contract symbol.
    symbol = "WRC20S",

    -- the number of decimals the token uses must be 8.
    -- means to divide the token amount by 100000000 to get its user representation.
    decimals = 8,

    -- the contract coin total supply, please update it with the actual contract symbol.
    -- note: the Maximum issued totalSupply is 90000000000 * 100000000
    totalSupply = 210000000 * 100000000
}

------------------------------------------------------------------------------------

-- internal method and table
_G.LibHelp={
    StandardKey={
        standard = "standard",
        owner = "owner",
        name = "name",
        symbol = "symbol",
        decimals = "decimals",
        totalSupply = "totalSupply",
    },
    OP_TYPE = {
        ADD_FREE = 1,
        SUB_FREE = 2
    },
    ADDR_TYPE = {
        REGID  = 1,
        BASE58 = 2
    },
    TableIsNotEmpty = function (t)
        return _G.next(t) ~= nil
    end,
    Unpack = function (t,i)
        i = i or 1
        if t[i] then
          return t[i], _G.LibHelp.Unpack(t,i+1)
        end
    end,
    LogMsg = function (msg)
        local logTable = {
             key = 0,
             length = string.len(msg),
             value = msg
       }
       _G.mylib.LogPrint(logTable)
    end,
    GetContractValue = function (key)
        assert(#key > 0, "Key is empty")
        local tValue = { _G.mylib.ReadData(key) }
        if _G.LibHelp.TableIsNotEmpty(tValue) then
          return true,tValue
        else
            _G.LibHelp.LogMsg("Key not exist")
          return false,nil
        end
    end,
    GetContractTxParam = function (startIndex, length)
        assert(startIndex > 0, "GetContractTxParam start error(<=0).")
        assert(length > 0, "GetContractTxParam length error(<=0).")
        assert(startIndex+length-1 <= #_G.contract, "GetContractTxParam length ".. length .." exceeds limit: " .. #_G.contract)
        local newTbl = {}
        for i = 1,length do
          newTbl[i] = _G.contract[startIndex+i-1]
        end
        return newTbl
    end,
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
        assert(_G.mylib.WriteOutAppOperate(appOperateTbl), "WriteAppData: ".. opType .." op err")
    end,
    GetFreeTokenCount = function (accountTbl)
        local freeMoneyTbl = { _G.mylib.GetUserAppAccValue( {idLen = #accountTbl, idValueTbl = accountTbl} ) }
        assert(_G.LibHelp.TableIsNotEmpty(freeMoneyTbl), "GetUserAppAccValue error")
        return _G.mylib.ByteToInteger(_G.LibHelp.Unpack(freeMoneyTbl))
    end,
    TransferToken = function (fromAddrTbl, toAddrTbl, moneyTbl)
        local money = _G.mylib.ByteToInteger(_G.LibHelp.Unpack(moneyTbl))
        assert(money > 0, money .. " <=0 error")
        local freeMoney = _G.LibHelp.GetFreeTokenCount(fromAddrTbl)
        assert(freeMoney >= money, "Insufficient money to transfer in the account.")

        _G.LibHelp.WriteAppData(_G.LibHelp.OP_TYPE.SUB_FREE, moneyTbl, fromAddrTbl)
        _G.LibHelp.WriteAppData(_G.LibHelp.OP_TYPE.ADD_FREE, moneyTbl, toAddrTbl)
    end,
    GetCurrTxAccountAddress = function ()
        return {_G.mylib.GetBase58Addr(_G.mylib.GetCurTxAccount())}
    end
}

-- contract method for caller
_G.ICO={
    TX_TYPE =
    {
      CONFIG = 0x11,
      SEND_ASSET = 0x16,
    },
    Transfer=function (toTbl,valueTbl)
        local base58Addr = _G.LibHelp.GetCurrTxAccountAddress()
        assert(_G.LibHelp.TableIsNotEmpty(base58Addr),"GetBase58Addr error")

        _G.LibHelp.TransferToken(base58Addr, toTbl, valueTbl)
    end,
    Config=function ()
        -- check contract statu
        assert(_G.LibHelp.GetContractValue("name")==false,"Already configured")

        -- write down standard key
        for k,v in pairs(_G.LibHelp.StandardKey) do
            if _G.Config[k] then
                local value = {}
                if type(_G.Config[k]) ==  "number" then
                    value = {_G.mylib.IntegerToByte8(_G.Config[k])}
                else
                    value = {string.byte(_G.Config[k],1,string.len(_G.Config[k])) }
                end
                local writOwnerTbl = {
                    key = v,
                    length = #value,
                    value = value
                }
                assert(_G.mylib.WriteData(writOwnerTbl),'can not issue tokens, failed to write the key='..v..' value='.._G.Config[k])
            else
                error('can not issue tokens, failed to read the key='..k)
            end
        end

        -- issue tokens
        local totalSupplyTbl =  {_G.mylib.IntegerToByte8(_G.Config.totalSupply)}
        _G.LibHelp.WriteAppData(_G.LibHelp.OP_TYPE.ADD_FREE, totalSupplyTbl,{string.byte(_G.Config.owner,1,string.len(_G.Config.owner))})
        _G.LibHelp.LogMsg("contract config success, name: ".._G.Config.name.."issuer: ".._G.Config.owner)
      end
}

------------------------------------------------------------------------------------

assert(#_G.contract >=4, "Parameter length error (<4): " ..#_G.contract)
assert(_G.contract[1] == 0xf0, "Parameter MagicNo error (~=0xf0): " .. _G.contract[1])

if _G.contract[2] == _G.ICO.TX_TYPE.CONFIG then
    _G.ICO.Config()
elseif _G.contract[2] == _G.ICO.TX_TYPE.SEND_ASSET and #_G.contract==2+2+34+8 then
    local pos = 5
    local toTbl = _G.LibHelp.GetContractTxParam(pos, 34)
    pos = pos + 34
    local valueTbl = _G.LibHelp.GetContractTxParam(pos, 8)
    _G.ICO.Transfer(toTbl,valueTbl)
else
    error(string.format("Method %02x not found or parameter error", _G.contract[2]))
end
```
  
## Deploy the smart contract
\*The smart contract can be published by any address, and the publisher doesn’t have to be the owner of the contract.

|Used API| Description|
| :---:          | :---                                               |
| registercontracttx    | create a register script transaction |

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"registercontracttx","params":["wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j","/tmp/lua/WRC20.lua",110000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"hash":"9db3b3e6497b493d87e29607cc526c86717a1d60b559957df5fb4cf997219a08"},"error":null,"id":"curltext"}
```

## check smart contract regid

|Used API| Description|
| :---:          | :---                                               |
| getcontractregid    |Get an object containing regid and script |

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractregid","params":["9db3b3e6497b493d87e29607cc526c86717a1d60b559957df5fb4cf997219a08"]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"regid:":"128711-1","script":"c7f601000100"},"error":null,"id":"curltext"}
```

## check assert

|Used API| Description|
| :---:          | :---   |
| getcontractaccountinfo    | Get the account coins in the contract  |

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractaccountinfo","params":["128711-1","wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j"]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"mAccUserID":"774d486b47514b4866344358645a52526f657a3862395967596e507a47786273336a","FreeValues":0,"FrozenFunds":[]},"error":null,"id":"curltext"}

```

## config contract

|Used API| Description|
| :---:          | :---   |
| callcontracttx    | calling smart contract to config info|

* Smart contract command

```
              +---------------+---------------------------+--------------+
              |  Magic number + Specified contract method + Reserved bit |
"f0110000" =  +---------------+------------------------------------------+
              |  0xf0         |  0x11                     | 0000         |
              +---------------+---------------------------+--------------+
```

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"callcontracttx","params":["wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j","128711-1",0,"f0110000",1000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"hash":"c7139740bce3a3acb60063d36b28fae4234352ef4b79ef8c8f8feccdaa929367"},"error":null,"id":"curltext"}

```

## check value by the key

|Used API| Description|
| :---:          | :---   |
| getcontractdata    | get the app data by given regid |


**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractdata","params":["128711-1","owner"]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"scritpid":"128711-1","key":"owner","value":"wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j"},"error":null,"id":"curltext"}
```

## check assert after the contract is configured

|Used API| Description|
| :---:          | :---   |
| getcontractaccountinfo    | Get the account coins in the contract |

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractaccountinfo","params":["128711-1","wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j"]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"mAccUserID":"774d486b47514b4866344358645a52526f657a3862395967596e507a47786273336a","FreeValues":21000000000000000,"FrozenFunds":[]},"error":null,"id":"curltext"}

```

## transfer assert

|Used API| Description|
| :---:          | :---   |
| callcontracttx    | calling smart contract to config info |


**example**
```
"f0160000774e5057717639627646436e4d6d3164646951644837665577556b3251677273324e0010a5d4e8000000"  =
+---------------------+------------------------------------+------------------------+---------------------------------------------------------------------+--------------------------------+
|Magic number（1 byte）+ Specified contract method(1 byte)  + Reserved bit（2 bytes）+ receiver:wNPWqv9bvFCnMm1ddiQdH7fUwUk2Qgrs2N(34 bytes)                + Amount:10000 token (8 bytes)  |
+---------------------+------------------------------------+------------------------+---------------------------------------------------------------------+--------------------------------+
|  0xf0               |  0x16                              | 0000                   |774e5057717639627646436e4d6d3164646951644837665577556b3251677273324e | 0010a5d4e8000000               |
+---------------------+------------------------------------+------------------------+---------------------------------------------------------------------+--------------------------------+

```

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"callcontracttx","params":["wMHkGQKHf4CXdZRRoez8b9YgYnPzGxbs3j","128711-1",0,"f0160000774e5057717639627646436e4d6d3164646951644837665577556b3251677273324e0010a5d4e8000000",1000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"hash":"78bf509e1c5ef915b7ffd2b4ae57a1f729ff99d1dacf4df5526427080e3ed25f"},"error":null,"id":"curltext"}

```

## check assert again
* Check the assets again to determine if the transfer was successful

**Example**
```json
curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractaccountinfo","params":["128711-1","wNPWqv9bvFCnMm1ddiQdH7fUwUk2Qgrs2N"]}' -H 'content-type:application/json;' http://127.0.0.1:6967
{"result":{"mAccUserID":"774e5057717639627646436e4d6d3164646951644837665577556b3251677273324e","FreeValues":1000000000000,"FrozenFunds":[]},"error":null,"id":"curltext"}
```

---

#### Task
* Deploy an ICO smart contract and transfer 10 tokens to the trainer account.