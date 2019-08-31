# 智能合约开发常用方法

<extoc></extoc>

---
## LOG_TYPE
Log 输出类型：`string`和`number`

```lua
LOG_TYPE =
{
    ENUM_STRING = 0,
    ENUM_NUMBER = 1
}
```

---

## ADDR_TYPE
账户格式类型

```lua
ADDR_TYPE = {
    REGID  = 1,
    BASE58 = 2
  }
```

---
## OP_TYPE
账户操作类型定义

```lua
OP_TYPE = {
    ADD_FREE 	= 1,
    SUB_FREE 	= 2
  }
```

---

## TableIsNotEmpty
判断传入的table是否为非空

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

`true` :不为空
`flase` : 为空

---
## Unpack
从第i个元素开始，遍历并输出table数组所有元素

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

`table数组所有元素`



---
## GetContractTxParam
获取合约内容

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

`目标合约内容`

---

## MemIsEqual
比较t1和t2是否相等

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

`true` :相等
`flase` : 不相等


---
## GetByte
获取字符串的byte table数组


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

`转换后的table数组`


---
## Serialize
table数组转换为字符串或者hex

`hex` = true : 转换为`hex`

`hex` = false : 转换为`字符串`

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

转换后的字符串或者hex - `String`

---
## Main
智能合约的入口函数
* Main 方法是智能合约调用的入口；
* 当调用智能合约时，table类型的contract会作为全局变量传入。
* 根据字节数组里面的数组第二个字段判定，应该走到哪一个不同的方法

```lua
Main = function()
--  for i=1, #contract do
--	print(i, contract[i])
-- end
  assert(#contract >=4, "Param length error (<4): " ..#contract )
  assert(contract[1] == 0xf0, "Param MagicNo error (~=0xf0): " .. contract[1])

  if contract[2] == 0x17 then
   -- 调用方法

  else
    error('method# '..string.format("%02x", contract[2])..' not found')
  end

end
```
---
