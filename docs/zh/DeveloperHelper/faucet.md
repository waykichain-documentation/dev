# 测试网水龙头
## 简介
开发者可以通过测试网水龙头获取测试币，每一次请求将获得10个测试网维基币

## 使用方法

* 使用链接：`https://faucet.wiccdev.org/testnet/getwicc/{address}`,将`{address}`换成相应的地址，即可获得10个测试链维基币

## 实例
```json
// Request
https://faucet.wiccdev.org/testnet/getwicc/wYXV7QzHZnb8LuWw7Xa24dfUTqmH2tNZBq

// Response
{"id":1,"jsonrpc":"2.0","result":{"hash":"49df0aa85954621da6487615fc24a0c40aa5842bc27cdaff10428b4a754717d5"}}
```
---

#### WaykiChain 公共节点
**Testnet 公共节点，仅供测试**

url: https://testnode.wiccdev.org

RPC username: wiccuser

RPC password: 123456

**example**
```
// Request
~$curl -u wiccuser:123456 -d '{"jsonrpc":"2.0","id":"curltext","method":"getinfo","params":[]}' -H 'content-type:application/json;' https://testnode.wiccdev.org

// Response
{
    "result": {
        "version": 1010101,
        "fullversion": "v1.1.1.1-ee173ce-release-linux (2019-01-23 10:37:43 +0800)",
        "protocolversion": 10001,
        "walletversion": 0,
        "balance": 1999749.99679999,
        "timeoffset": 0,
        "proxy": "",
        "nettype": "TEST_NET",
        "genblock": 0,
        "chainwork": "000000000000000000000000000000000000000000000000000000000000876e",
        "tipblocktime": 1548490510,
        "paytxfee": 0.0001,
        "relayfee": 0.00001,
        "fuelrate": 1,
        "fuel": 0,
        "data directory": "/root/.WaykiChain/testnet",
        "tip block hash": "c28bef6d3029ba6149235f27eb482a67d6202194a3d139b3a2e0bc59159aea8f",
        "syncheight": 34670,
        "blocks": 34670,
        "connections": 4,
        "errors": ""
    },
    "error": null,
    "id": "curltext"
}

~$
```

