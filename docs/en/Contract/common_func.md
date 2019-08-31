# Common methods for smart contract development

* **Waykichain Smart Contracts Developed in `Lua` Language**

<extoc></extoc>

---
## LOG\_TYPE
Type of log

```lua
LOG_TYPE =
{
    ENUM_STRING = 0,
    ENUM_NUMBER = 1
}
```

---
## ADDR\_TYPE
Type of format of the account

```lua
ADDR_TYPE = {
    REGID  = 1,
    BASE58 = 2
  }
```

---
## OP\_TYPE
Operation type

```lua
OP_TYPE = {
    ADD_FREE    = 1,
    SUB_FREE    = 2
  }
```

---

## TableIsNotEmpty
Determine if the data is non-empty

**Code sample**
```lua
TableIsNotEmpty = function (t)
    return _G.next(t) ~= nil
end
```

**Parameters**

| parameter | type  | example     |
|:---------:|:-----:| ----------- |
|     t     | table | {0x01,0x23} |

**Returns**

`true` :non-empty
`flase` : empty

---
## Unpack
Get all of element in the table

**Code sample**
```lua
Unpack = function (t,i)
    i = i or 1
    if t[i] then
      return t[i], Unpack(t,i+1)
    end
end
```

**Parameters**

| parameter |  type  | example       |
|:---------:|:------:| ------------- |
|     t     | table  | {0x01,0x23}   |
|     i （optional）    | Number | 5 (Default:1) |


**Returns**

`all of element in the table`



---
## GetContractTxParam
Get smart contract content

**Code sample**
```lua
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
```

**Parameters**

| parameter  |  type  | example |
|:----------:|:------:| ------- |
| startIndex | Number | 2       |
|   length   | Number | 5       |


**Returns**

smart contract content

---

## MemIsEqual
Compare whether t1 and t2 are equal

**Code sample**
```lua
MemIsEqual = function (t1,t2)
    assert(TableIsNotEmpty(t1), "t1 is empty")
    assert(TableIsNotEmpty(t2), "t2 is empty")

    if(#t1 ~= #t2) then
      return false
    end

    local i = 1
    for i = #t1,1,-1 do
      if t1[i] ~= t2[i] then
        return false
      end
    end
    return true
end
```

**Parameters**

| parameter | type  | example          |
|:---------:|:-----:| ---------------- |
|    t1     | table | {0x01,0x02,0x03} |
|    t2     | table | {0x01,0x02,0x03} |

**Returns**

`true` :equal
`flase` : not equal


---
## GetByte
Convert a string to an byte table

**Code sample**
```lua
GetByte = function(param)
  return {string.byte(param,1,string.len(param))}
end
```

**Parameters**

| parameter | type  | example          |
|:---------:|:-----:| ---------------- |
|    param     | String | "admin" |


**Returns**

`byte table`


---
## Serialize
Convert a table to String

**Code sample**
```lua
Serialize = function(obj, hex)
    local lua = ""
    local t = type(obj)

    if t == "table" then
        for i=1, #obj do
            if hex == false then
                lua = lua .. string.format("%c",obj[i])
            elseif hex == true then
                lua = lua .. string.format("%02x",obj[i])
      else
        error("index type error.")  
            end
        end
    elseif t == "nil" then
        return nil
    else
        error("can not Serialize a " .. t .. " type.")
    end

    return lua
end
```

**Parameters**

| parameter | type  | example          |
|:---------:|:-----:| ---------------- |
|    obj     | table | {0x01,0x02,0x03} |
|    hex     | boolean| false |

**Returns**

`Destination string` - `String`

---
## Main
Entry function of the smart contract
* The Main method is the entry for the caller;
* When calling a smart contract, a contract of type table is passed in as a global variable.
* According to the second field of the data in the byte array, which method should go to a different method

```lua
Main = function()
--  for i=1, #contract do
--  print(i, contract[i])
-- end
  assert(#contract >=4, "Param length error (<4): " ..#contract )
  assert(contract[1] == 0xf0, "Param MagicNo error (~=0xf0): " .. contract[1])

  if contract[2] == 0x17 then
   -- calling+ specific methods

  else
    error('method# '..string.format("%02x", contract[2])..' not found')
  end

end
```
---