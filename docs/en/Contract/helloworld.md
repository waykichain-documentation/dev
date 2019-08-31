Open up the online IDE in a new tab to start creating our Hello World contract.

* Online IDE: [WaykiMix][7] 
<br>

WaykiMix is online IDE which runs on the [local test network node][8].

## WaykiChain DAPP Foundation
The blockchain can be abstracted into a virtual machine that is synchronized across the network. A smart contract is code that executes on a blockchain system and changes the state in the virtual machine through transactions. Due to the characteristics of the blockchain, the call of the smart contract can be guaranteed to be serial and globally consistent.

WaykiChain smart contract currently supports `Lua` language development, and does not support multi-contract calls. This means a contract cannot call a contract. 

### Key words

| Keywords | Description |
| :-- | :-- |
| contract | The default global variable name inside the smart contract, used to get external call context inside the smart contract, type is table |
| [mylib][1] | smart contract internal API interface |
| [Common Methods][2] | common methods for smart contract development, based on Lua language |
| [mylib use case][3] | common use cases based on `mylib` package |
| [WaykiMix][4] | smart contract development online IDE |
| [WaykiBridge][5] | DAPP wallet interface unification tool |
| [BaaS][6] | WaykiChain public API|

## Debug environment configuration
WaykiChain smart contracts can be development through [WaykiMix][7]. WaykiMix is online IDE which runs on the [local test network node][8].

## Smart contract basic structure
**`demo.lua`**
```lua
Mylib = require "mylib"

Main = function()

End

Main()
```

---
## Helloworld Smart Contract Source Code
The entry to the smart contract is the `Main` function, and the `Main` function contains two different functional methods.

`SayHelloToLog()` is used to output `Hello World!` in the log.

`SaveHelloToChain()` is used to write `Hello`-`World` to `LevelDB` in the blockchain as `key-value`, which can be found by a [BaaS][9] Querying the corresponding value.

```lua
mylib = require "mylib"
--must start with mylib = require "mylib". Be sure to put it in the first line. If the first line is left blank, an exception will be reported.

--Define calling smart contract events
METHOD = {
    CHECK_HELLOWORLD  = 0x17,
    SEND_HELLOWORLD = 0x18
}

LOG_TYPE =
{
    ENUM_STRING = 0,
    ENUM_NUMBER = 1
}

----Used to print log information to a file
LogMsg = function(LogType,Value)
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

--Write date into the blockChain
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

--get external call context
GetContractTxParam = function (startIndex, length)
    assert(startIndex > 0, "GetContractTxParam start error(<=0).")
    assert(length > 0, "GetContractTxParam length error(<=0).")
    assert(startIndex+length-1 <= #contract, "GetContractTxParam length ".. length .." exceeds limit: " .. #contract)

    local newTbl = {}
    local i = 1
    for i = 1,length do
        newTbl[i] = contract[startIndex+i-1]
    end
    return newTbl
end

---------------------------------------------------

SayHelloToLog = function()
    LogMsg(LOG_TYPE.ENUM_STRING,"Hello World!")
end

SaveHelloToChain = function(contextTbl)
    WriteStrkeyValueToDb("Hello",contextTbl)
    LogMsg(LOG_TYPE.ENUM_STRING,"Save Hello To Chain Successfully!")
end

--Entry function of smart contract
Main = function()
  assert(#contract >=4, "Param length error (<4): " ..#contract )
  assert(contract[1] == 0xf0, "Param MagicNo error (~=0xf0): " .. contract[1])

  if contract[2] == METHOD.CHECK_HELLOWORLD then
    SayHelloToLog()
  elseif contract[2] == METHOD.SEND_HELLOWORLD then
    local contextTbl = GetContractTxParam(5,5)--get external call context==“World”
    SaveHelloToChain(contextTbl)
  else
    error('method# '..string.format("%02x", contract[2])..' not found')
  end
end

Main()
```
---

### Deploying smart contract
#### Deploy smart contract to testnet via [WaykiMix][10]
![][image-1]
#### Deployed smart contract  to testnet via the local node RPC [registercontracttx][11]

```json
// Request
Curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"registercontracttx","params":["wLWxCRWDTb3fUTa6ztoTTCzFwDqzbckSJ7","/tmp/lua/helloworld .lua",110000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "hash":"dfdb78278c486016a8588c6ac525eced4348725f5a85af51c898fed320e72e23"
    },
    "error": null,
    "id": "curltext"
}
```

### Get smart contract regid
#### Get smart contract regid by [WaykiMix][12]
![][image-2]
#### Get smart contract regid by local node RPC [getcontractregid][13]

```json
// Request
Curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"getcontractregid","params":["dfdb78278c486016a8588c6ac525eced4348725f5a85af51c898fed320e72e23"]}' -H 'content- Type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
        "regid:":"320561-1",
        "regid_hex" : "31e404000100"
    },
    "error": null,
    "id": "curltext"
}
```

### Calling Smart Contract 1
Here the contract is passed in the parameter `f0170000`. According to the contract source, the `SayHelloToLog()` method is called, ie \`Hello World!" is printed in the log.
**Parameter parsing of the calling contract**
```
               +-------------+---------------------------+---------------+
               |Magic number + specified contract method | Reserved bits |
"f0170000" =   +-------------+---------------------------+---------------+
               | 0xf0        | 0x17                      | 0000          |
               +-------------+---------------------------+---------------+
```
#### Calling Smart Contracts via [WaykiMix][14]
![][image-3]
#### Calling a smart contract via the local node RPC [callcontracttx][15]

```json
// Request
Curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"callcontracttx","params":["wLWxCRWDTb3fUTa6ztoTTCzFwDqzbckSJ7","320561-1",0 ,"f0170000",1000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
       "hash":"f867200b3d91fd2cbf8b0d8b8840e64bf620f08923acc3126fb960dd04b3064f"
    },
    "error": null,
    "id": "curltext"
}
```
#### View log
The premise is to add `debug=vm` to the local node `WaykiChain.conf` to view the `vm.log` information in the corresponding directory.
The screenshot shows that the log information is the running content of the Check method in the contract, that is, output `Hello World!` information to the `vm.log` file.
![][image-4]

### Calling Smart Contract 2
Here the contract is passed in the parameter `f0180000576f726c64`, and the `SAVEHELLOTOCHAIN()` method is called according to the contract source.
The 5 bytes of the parameters of the calling contract are taken as the `value` value, and "Hello" is written as the `key` value in the `LevelDB` of the blockchain.

**Parameter parsing of the calling contract**
```
                         +------------+---------------------------+---------------+------------+
                         |Magic number| specified contract method | Reserved bits | "World"    |
"f0180000576f726c64" =   +------------+---------------------------+---------------+------------+
                         |0xf0        | 0x18                      | 0000          | 576f726c64 |
                         +------------+---------------------------+---------------+------------+
```

#### Calling Smart Contracts via [WaykiMix][16]
![][image-5]
#### Calling a smart contract via the local node RPC [callcontracttx][17]
```json
// Request
Curl -u Waykichain:admin -d '{"jsonrpc":"2.0","id":"curltext","method":"callcontracttx","params":["wLWxCRWDTb3fUTa6ztoTTCzFwDqzbckSJ7","320561-1",0 ,"f0180000576f726c64",1000000]}' -H 'content-type:application/json;' http://127.0.0.1:6967

// Response
{
    "result":
    {
       "hash":"f65fc179dd9056b06a4a843ee696afa115678bdad523440ae6facd98e7d61a9d"
    },
    "error": null,
    "id": "curltext"
}
```
#### Query key-value value with [BaaS][18]
![][image-6]

[1]:	contract_api.md
[2]:	common_func.md
[3]:	api_debug.md
[4]:	../DeveloperHelper/waykimix.md
[5]:	../DeveloperHelper/application_api.md
[6]:	../DeveloperHelper/baas.md
[7]:	https://waykimix.wiccdev.org 
[8]:	../NodeDeployment/testnet.md
[9]:	../DeveloperHelper/baas.md
[10]:	../DeveloperHelper/waykimix.md
[11]:	../JsonRpcApi/contract.md#registercontracttx
[12]:	../DeveloperHelper/waykimix.md
[13]:	../JsonRpcApi/contract.md#getcontractregid
[14]:	../DeveloperHelper/waykimix.md
[15]:	../JsonRpcApi/contract.md#callcontracttx
[16]:	../DeveloperHelper/waykimix.md
[17]:	../JsonRpcApi/contract.md#callcontracttx
[18]:	../DeveloperHelper/baas.md

[image-1]:	../images/deploy.png
[image-2]:	../images/getcontractregid.png
[image-3]:	../images/call1.png
[image-4]:	../images/vmlog.png
[image-5]:	../images/call2.png
[image-6]:	../images/getcontractbybaas.png
